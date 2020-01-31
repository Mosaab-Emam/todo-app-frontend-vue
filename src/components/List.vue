<template>
  <v-card elevation="6">
    <template v-for="(todo, index) in todos">
      <v-list-item :key="todo.id">
        <v-list-item-content>
          <v-list-item-title class="d-flex align-center">
            <template v-if="!todo.editMode">
              <v-checkbox
                v-model="todo.done"
                @change="updateDoneState(todo.id, todo.name, todo.done)"
                dense
                hide-details
              ></v-checkbox>
              {{ todo.name }}
            </template>
            <v-form v-else @submit.prevent="updateTodoName(todo.id, todo.done)">
              <v-text-field
                v-model="todoName"
                autofocus
                placeholder="Input todo name then hit Enter"
              ></v-text-field>
            </v-form>
            <div class="chips ml-auto">
              <v-chip
                v-ripple
                @click="toggleEditMode(todo.name, todo.id)"
                color="black"
                text-color="white"
              >
                <v-icon class="material-icons" left>mdi-pencil</v-icon>
                Edit
              </v-chip>
              <v-chip
                v-ripple
                @click="deleteTodo(todo.id)"
                color="red"
                text-color="white"
              >
                <v-icon left>mdi-close</v-icon>
                Delete
              </v-chip>
            </div>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-divider :key="index"></v-divider>
    </template>
  </v-card>
</template>

<script>
import todosQuery from "@/apollo queries/todos.gql"
import updateTodoQuery from "@/apollo queries/updateTodo.gql"
import deleteTodoQuery from "@/apollo queries/deleteTodo.gql"

export default {
  name: "List",
  apollo: {
    todos: {
      prefetch: true,
      query: todosQuery,
      update: (data) => {
        for (let todo of data.todos) {
          todo.editMode = false
        }
        return data.todos
      }
    }
  },
  data() {
    return {
      todoName: ""
    }
  },
  methods: {
    toggleEditMode(name, id) {
      this.todoName = name
      let found = this.todos.find((element) => {
        return element.id === id
      })
      found.editMode = true
    },
    updateTodoName(id, done) {
      this.$apollo.mutate({
        mutation: updateTodoQuery,
        variables: {
          id,
          name: this.todoName,
          done
        }
      })
      let found = this.todos.find((element) => {
        return element.id === id
      })
      found.name = this.todoName
      found.editMode = false
      this.todoName = ""
    },
    updateDoneState(id, name, done) {
      this.$apollo.mutate({
        mutation: updateTodoQuery,
        variables: {
          id,
          name,
          done
        }
      })
    },
    deleteTodo(id) {
      this.$apollo.mutate({
        mutation: deleteTodoQuery,
        variables: {
          id
        },
        refetchQueries: [{ query: todosQuery }]
      })
    }
  }
}
</script>

<style scoped>
.v-list-item__title {
  padding-left: 8px;
}
.v-input--selection-controls {
  margin-top: 0;
}
.v-chip {
  margin-left: 1rem;
}
.v-chip i {
  margin-right: 6px;
}
</style>
