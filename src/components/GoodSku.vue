<template>
  <ul class="sku-list">
    <li v-for="(sku, skuIndex) in skuList" :key="sku.uuid">
      <i
        class="iconfont btn-del"
        v-show="skuList.length > 1"
        @click="handleDelSku(skuIndex)"
      >&#xe68b;</i>

      <div class="sku-name">
        <h4>规格名</h4>
        <el-input v-model="sku.skuName"></el-input>
      </div>

      <div class="sku-option">
        <h4>规格值</h4>
        <ul class="option-list">
          <li v-for="(option, index) in sku.skuOption" :key="index">
            <el-input v-model="option.optionName"></el-input>
            <i
              class="iconfont btn-del"
              v-show="sku.skuOption.length > 1"
              @click="handleDelOption(sku.skuOption, index)"
            >&#xe68b;</i>
          </li>
          <li>
            <el-button
              v-show="sku.skuOption.length < 3"
             @click="handleAddOption(sku.skuOption)"
            >
              +新增规格值
            </el-button>
          </li>
        </ul>
      </div>
    </li>
    <el-button
      v-show="skuList.length < 3"
      @click="handleAddSku"
    >新增其他规格</el-button>
  </ul>
</template>

<script>
export default {
  name: 'GoodSku',
  data() {
    return {
      skuList:[
        {
          skuName:'',
          skuOption: [
            {optionName: ''}
          ]
        }
      ]
    }
  },
  watch: {
    skuList: {
      handler(newVal) {
        this.$emit('change', newVal)
      },
      deep: true
    }
  },
  methods: {
    // 新增sku
    handleAddSku() {
      this.skuList.push({
        skuName: '',
        skuOption: [
          { optionName: '' }
        ]
      })
    },
    // 删除sku
    handleDelSku(skuIndex) {
      this.skuList.splice(skuIndex, 1);
    },
    // 新增规格值
    handleAddOption(skuOption) {
      skuOption.push({ optionName: '' })
    },

    handleDelOption(skuOptions, index) {
      skuOptions.splice(index, 1)
    }
  }
}
</script>

<style lang="less" scoped>
.el-input {
  width: 200px;
}
.sku-list {
  width: 800px;
  margin: 0 auto;

  > li {
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
    position: relative;
    background-color: #f7f7f7;

    &:hover {
      > .btn-del {
        display: block;
      }
    }

    .btn-del {
      display: none;
      font-size: 12px;
      width: 20px;
      height: 20px;
      border-radius: 10px;
      position: absolute;
      right: -10px;
      top: -10px;
      background-color: #858598;
      font-size: 12px;
      color: #fff;
      line-height: 20px;
      cursor: pointer;
    }

    .sku-name, .sku-option {
      display: flex;
      align-items: center;
      padding-bottom: 10px;

      h4 {
        padding-right: 10px;
        font-weight: normal;
      }

      .option-list {
        flex: 1;
        display: flex;
        flex-wrap: wrap;
        li {
          position: relative;
          
          &:hover {
            > .btn-del {
              display: block;
            }
          }

          .btn-del {
            transform: scale(0.6);
          }

          &:not(:first-child) {
            padding-left: 10px;
          }
        }
      }
    }
  }
}
</style>