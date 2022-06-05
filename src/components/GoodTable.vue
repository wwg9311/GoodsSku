<template>
  <div>
    <el-table
      :data="tableData"
      :span-method="objectSpanMethod"
    >
      <el-table-column
        v-for="column in columns" :key="column.prop"
        :prop="column.prop"
        :label="column.label"
      >
        <template
          v-if="['count', 'price'].includes(column.prop)"
          #default="{row}"
        >
          <el-input v-model="row[column.prop]"></el-input>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'GoodTable',
  props: {
    skuList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      tableData: [],
      columns:[],
      spanData:[]
    }
  },
  watch:{
    skuList: {
      handler(){
        this.initData()
      }
    }
  },
  methods: {
    objectSpanMethod({ rowIndex, columnIndex }) {
      const spanNum = this.spanData[columnIndex]
      if (spanNum) {
        if (rowIndex % spanNum === 0) {
          return {
            rowspan: spanNum,
            colspan: 1
          };
        }else {
          return {
            rowspan: 0,
            colspan: 0
          }
        }
      }
    },
    // 初始化表格列
    initTableColumn() {
      const columns = [];
      this.skuList.forEach((sku, index) => {
        columns.push({label: sku.skuName, prop: 'field' + index, width: 100,})
      })
      columns.push(
        {label: '库存', prop: 'count'},
        {label: '价格', prop: 'price'}
      )
      
      this.columns = columns;
    },
    // 初始化表格数据
    initTableData() {
      let result = [];
      for(let i = 0; i < this.skuList.length; i++) {
        result = this.formateData(this.skuList[i].skuOption, i, result);
      }
      this.tableData = result;
    },
    // 格式化商品规格
    formateData(list, index, result = []) {
      let newResult = [];

        result.forEach((item) => {
          list.forEach((skuOption) => {
            newResult.push({...item, ['field'+index]: skuOption.optionName})      
          })
        })
      
        if(!result.length) {
          list.forEach(skuOption => {
            newResult.push({['field'+index]: skuOption.optionName, count: '', price: ''})
          })
        }

      return newResult
    },
    // 初始化表格每一列的每一行占据几行
    initSpan() {
      this.skuList.forEach((item, index) => {
        // 每一列的每一行占据的行数 = 其后规格数量的乘积
        this.spanData[index] = this.skuList.slice(index+1).reduce((cur, next) => cur * next.skuOption.length, 1) || 1
      })
    },
    // 初始化基本数据
    initData() {
      this.initSpan();
      this.initTableColumn();
      this.initTableData();
    }
  },

}
</script>

<style lang="less" scoped>

</style>