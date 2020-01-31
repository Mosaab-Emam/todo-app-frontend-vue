<template>
  <div class="home">
    <h1>Testing graphql server locally</h1>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        ID: {{ todo.id }} - Name: {{ todo.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "home",
  apollo: {
    todos: gql`
      {
        todos {
          id
          name
        }
      }
    `
  }
};
</script>
