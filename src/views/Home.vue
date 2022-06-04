<template>
  <div class="home">
    <good-sku @change="handleChangeSku"/>
    <good-table :sku-list="skuList" />
  </div>
</template>

<script>
import GoodSku from '@/components/GoodSku';
import GoodTable from '@/components/GoodTable';
import {cloneDeep} from 'lodash'

export default {
  name: "Home",
  components: { GoodSku, GoodTable },
  data() {
    return {
      skuList: []
    }
  },
  methods: {
    // 更改sku配置
    handleChangeSku(skuList) {
      this.skuList = cloneDeep(skuList);
    }
  }
};
</script>

<style lang="less" scoped>
.home {
  margin: 0 auto;
  padding: 20px 0;
  width: 1200px;
}
</style>
