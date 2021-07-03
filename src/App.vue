<template>
  <div id="app">
    <editable-table></editable-table>

  </div>
</template>

<script>
import EditableTable from './views/editable-table'

export default {
  name: 'App',
  components: {
    EditableTable,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
