<template>
  <div id="app">
    <h2>Editable Table</h2>
    <editable-table></editable-table>
    <h2>Inputable Table</h2>
    <editable-table-plus></editable-table-plus>
  </div>
</template>

<script>
import EditableTable from './views/editable-table'
import EditableTablePlus from  './views/editable-table-plus'
export default {
  name: 'App',
  components: {
    EditableTable,
    EditableTablePlus
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
