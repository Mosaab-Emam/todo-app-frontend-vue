<template>
  <v-toolbar elevation="6">
    <v-toolbar-title>Title</v-toolbar-title>
    <v-spacer></v-spacer>
    <v-chip
      v-ripple
      v-if="!addForm"
      @click="
        {
          addForm = true
        }
      "
      color="light-blue"
      text-color="white"
    >
      <v-icon left>mdi-plus</v-icon>
      Add todo
    </v-chip>
    <v-form v-else @submit.prevent="addTodo()">
      <v-text-field
        v-model="newTodoName"
        autofocus
        placeholder="Input todo name then hit Enter"
      ></v-text-field>
    </v-form>
  </v-toolbar>
</template>

<script>
import todosQuery from "@/apollo queries/todos.gql"
import addTodoQuery from "@/apollo queries/addTodo.gql"

export default {
  name: "Toolbar",
  data() {
    return {
      addForm: false,
      newTodoName: ""
    }
  },
  methods: {
    addTodo() {
      this.$apollo.mutate({
        mutation: addTodoQuery,
        variables: {
          name: this.newTodoName
        },
        refetchQueries: [{ query: todosQuery }]
      })
      this.newTodoName = ""
      this.addForm = false
    }
  }
}
</script>

<style>
.v-chip:hover {
  cursor: pointer;
}

.v-text-field {
  width: 16rem;
}

.v-input__slot {
  margin-bottom: 0 !important;
}

.v-text-field__details {
  display: none !important;
}
</style>
