<template>
  <div>
    <Row style="text-align:right">
      <Button @click="initData" style="padding-right: 8px">重置</Button>
      <Button @click="printData" style="padding-right: 8px">打印</Button>
      <Button @click="addData" style="padding-right: 8px">添加</Button>
      <Button @click="removeData">删除</Button>
    </Row>
    <Row>
      <Col span="18">
        <Table :columns="columns" :data="data" @on-row-click="handleRowClick" highlight-row>
          <template slot-scope="{row, index}" :slot="'name'">
            <Input v-model="data[index].name" style="width:100%" :disabled="disabled" />
          </template>
          <template slot-scope="{row, index}" :slot="'dataType'">
            <Select :transfer="true" v-model="data[index].dataType" style="width:100%" @on-change="handleDataTypeChange">
              <Option :transfer="true" v-for="(option, index) in dataTypes" :key="index" :label="option.typeName" :value="option.typeName"></Option>
            </Select>
          </template>
          <template  slot-scope="{row, index}" :slot="'isPk'">
            <Checkbox v-model="data[index].isPk" :disabled="disabled"></Checkbox>
          </template>
          <template slot-scope="{row, index}" :slot="'allowNull'">
            <Checkbox slot="content" v-model="data[index].allowNull" :disabled="disabled"></Checkbox>
          </template>
        </Table>
      </Col>
      <Col span="6">
        <Form :model="columnData" :label-width="60">
          <FormItem label="名称">
            <Input v-model="columnData.name"/>
          </FormItem>
          <FormItem label="长度">
            <Input :disabled="!columnOption.resetLength" v-model="columnData.length" placeholder="可输入长度" />
          </FormItem>
          <FormItem label="精度">
            <Input :disabled="!columnOption.resetScale" v-model="columnData.scale" placeholder="可输入精度" />
          </FormItem>
          <FormItem label="默认值">
            <Input v-model="columnData.defaultVal" placeholder="可输入默认值" />
          </FormItem>
        </Form>
      </Col>
    </Row>
  </div>
</template>
<script>
export default {
  data () {
    return {
      index: 0, // highlight index
      disabled: false, // disable the column Form
      data: [],
      columns: [
        {
          type: 'index',
          width: 60
        },
        {
          title: '名称',
          slot: 'name',
          minWidth: 100
        },
        {
          title: '类型',
          slot: 'dataType',
          minWidth: 100
        },
        {
          title: '是否主键',
          slot: 'isPk',
          minWidth: 60
        },
        {
          title: '允许空值',
          slot: 'allowNull',
          minWidth: 60
        }
      ],
      defaultData: {
        name: 'newColumn1',
        dataType: 'VARCHAR',
        length: null,
        scale: null,
        isPk: false,
        allowNull: false,
        defaultVal: ''
      },
      dataTypes: [ {
        "typeName" : "INTEGER",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "SMALLINT",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "NUMERIC",
        "resetLength" : true,
        "resetScale" : true
      }, {
        "typeName" : "DECIMAL",
        "resetLength" : true,
        "resetScale" : true
      }, {
        "typeName" : "CHAR",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "VARCHAR",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "DATE",
        "resetLength" : false,
        "resetScale" : false
      }, {
        "typeName" : "TIMESTAMP",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "BLOB",
        "resetLength" : true,
        "resetScale" : false
      }, {
        "typeName" : "TEXT",
        "resetLength" : false,
        "resetScale" : false
      } ],
      columnData: {},
      columnOption: {}
    }
  },
  methods: {
    addData () {
      this.clearHighlight()
      this.data.push({
        ...this.defaultData,
        name: `newColumn${this.data.length + 1}`
      })
      this.setIndex(this.data.length - 1)
      this.setColumnForm()
      this.setHighlightRow()
    },
    removeData () {
      if (this.data.length <= 1) {
        return this.$Message.error({
          content: `至少保留一行数据`
        })
      }

      this.data.splice(this.index, 1)
      this.setIndex(this.index)
      this.setHighlightRow()
    },
    printData () {
      // clone the data
      let result = this.data.map(item => {
        let obj = {}
        Object.keys(this.defaultData).forEach(key => {
          obj[key] = item[key]
        })
        return obj
      })

      console.table(result)
    },
    initData () {
      this.data = [{...this.defaultData}]
      this.setIndex(0)
      this.setColumnForm()
      this.setHighlightRow()
    },
    handleRowClick (data, row) {
      this.setIndex(row)
      this.clearHighlight()
      this.setHighlightRow(row)
      this.setColumnForm(row)
      console.log(`cliked ${row}th row`)
    },
    handleDataTypeChange (value) {
      this.setColumnForm()
      console.log('handleDataTypeChange', value)
    },
    setIndex (index) {
      this.index = index <= this.data.length - 1 ? index : this.data.length - 1
    },
    setColumnForm (index) {
      index = index || this.index
      this.columnData = this.data[index] || {}
      this.columnOption = this.dataTypes.find(item => item.typeName === this.columnData.dataType) || {}
      // TODO if has limit reset column data value
      if (!this.columnOption.resetScale) { // if disable scale, reset scale equals null
        this.columnData.scale = null
      }
      if (!this.columnOption.resetLength) { // if disable length, reset length equals null
        this.columnData.length = null
      }
    },
    setHighlightRow (index) {
      this.index = typeof index === 'number' ? index : this.data.length - 1
      this.data[this.index]._highlight = true
    },
    clearHighlight () {
      this.data.forEach(item => {
        if('_highlight' in item) {
          delete item._highlight
        }
      })
    }
  },
  created () {
    this.initData()
  }
}
</script>

<style scoped>

</style>
