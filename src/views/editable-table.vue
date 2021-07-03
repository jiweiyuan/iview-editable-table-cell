<template>
  <Table :columns="columns" :data="data">
    <template  v-for="(column, index) of dataColumns" slot-scope="{row}" :slot="column.slot">
      <div :key="index">
        <editable-table-cell :meta="{row: row, col: column}" :show-input="row.editMode" v-model="row[column.slot]" @on-save="handleSave">
          <span slot="content">{{ row[column.slot] }}</span>
        </editable-table-cell>
      </div>
    </template>
  </Table>
</template>
<script>
import EditableTableCell from '../components/editable-table-cell'
export default {
  components: {
    EditableTableCell
  },
  data () {
    return {
      columns: [
        {
          type: 'selection',
          slot: 'selection',
          width: 60
        },
        {
          title: '姓名',
          slot: 'name'
        },
        {
          title: '年龄',
          slot: 'age'
        },
        {
          title: '出生日期',
          slot: 'birthday'
        },
        {
          title: '地址',
          slot: 'address'
        }
      ],
      baseColumns: [],
      dataColumns: [{
        title: '姓名',
        slot: 'name'
      },
        {
          title: '年龄',
          slot: 'age'
        },
        {
          title: '出生日期',
          slot: 'birthday'
        },
        {
          title: '地址',
          slot: 'address'
        }],
      data: [
        {
          name: '王小明',
          age: 18,
          birthday: '919526400000',
          address: '北京市朝阳区芍药居'
        },
        {
          name: '张小刚',
          age: 25,
          birthday: '696096000000',
          address: '北京市海淀区西二旗'
        },
        {
          name: '李小红',
          age: 30,
          birthday: '563472000000',
          address: '上海市浦东新区世纪大道'
        },
        {
          name: '周小伟',
          age: 26,
          birthday: '687024000000',
          address: '深圳市南山区深南大道'
        }
      ]
    }
  },
  methods: {
    handleSave(val, {col={}, row={}}) {
      const message = `${row.name} 的 ${col.title} 修改为 ${val}`
      this.$Message.info({content: message})
    }
  },
  mounted() {

  }
}
</script>
