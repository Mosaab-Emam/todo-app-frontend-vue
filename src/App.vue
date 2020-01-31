<template>
  <v-app>
    <v-content>
      <v-container>
        <Toolbar />
        <List />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import Toolbar from "@/components/Toolbar"
import List from "@/components/List"

export default {
  components: {
    Toolbar,
    List
  }
}
</script>

<style>
.v-toolbar {
  margin-bottom: 1rem;
}
</style>
