<template>
  <div class="page-test2">
    <div
      class="list"
      v-for="(list,pindex) in attribute_arr"
      :key="pindex"
    >
      <div class="list-title">{{list.attribute_name}}</div>
      <div class="list-body flex">
        <a
          :class="{'value-item':true,'active':selectArr.join('').indexOf(item.attribute_value_id)!=-1,'disabled':noStockArr.join('').indexOf(item.attribute_value_id)!=-1}"
          v-for="(item,index) in list.child"
          :key="index"
          @click="chooseSku(item,pindex,index)"
        >{{item.attribute_value}}</a>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      attribute_arr: [
        {
          attribute_name: "颜色",
          child: [
            {
              attribute_value_id: "1",
              attribute_value: "玫瑰金"
            },
            {
              attribute_value_id: "2",
              attribute_value: "磨砂黑"
            }
          ]
        },
        {
          attribute_name: "内存",
          child: [
            {
              attribute_value_id: "4",
              attribute_value: "64G"
            },
            {
              attribute_value_id: "5",
              attribute_value: "128G"
            }
          ]
        },
        {
          attribute_name: "版本",
          child: [
            {
              attribute_value_id: "7",
              attribute_value: "移动定制版"
            },
            {
              attribute_value_id: "8",
              attribute_value: "联通定制版"
            }
          ]
        }
      ],
      all_att_arr: [
        {
          id: 1, //  这是sku_id
          stock: 0, //..这是这个skuid下的库存数量
          value: "1,4,7",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 0, //..这是这个skuid下的库存数量
          value: "1,4,8",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 12, //..这是这个skuid下的库存数量
          value: "1,5,7",
          is_check: true //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 15, //..这是这个skuid下的库存数量
          value: "1,5,8",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 15, //..这是这个skuid下的库存数量
          value: "2,4,7",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 1, //..这是这个skuid下的库存数量
          value: "2,4,8",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 15, //..这是这个skuid下的库存数量
          value: "2,5,7",
          is_check: false //  可以定义默认选择规格
        },
        {
          id: 1, //  这是sku_id
          stock: 15, //..这是这个skuid下的库存数量
          value: "2,5,8",
          is_check: false //  可以定义默认选择规格
        }
      ],
      selectArr: [], //  已选择的组合
      noStockArr: [], //  无库存组合
      hasStockArr: [] // 有库存组合
    };
  },
  mounted() {},
  methods: {
    chooseSku(spec, pindex, lindex) {
      let check_id = spec.attribute_value_id; //  当前选择id
      // 高亮当前选择
      if (this.selectArr[pindex] != check_id) {
        this.$set(this.selectArr, pindex, check_id);
      } else {
        this.$set(this.selectArr, pindex, false);
      }
      // 灰显无货操作

      let noStock = []; //  临时数组==> 无库存数组
      let hasStock = [];    //  临时数组==> 有库存数组
      let no_stock_arr = [];    //  临时数组==> 灰显数组

      this.all_att_arr.filter(item => {
        if (item.value.match(check_id)) {
          if (item.stock <= 0) {
            //    无库存
            let str = item.value.split(",");
            str.filter(v => {
              noStock.push(v);
            });
          } else {
            //    有库存
            let str = item.value.split(",");
            str.filter(v => {
              hasStock.push(v);
            });
          }
        }
      });
      // 对noStock与hasStock取差集
      noStock = new Set(noStock);
      hasStock = new Set(hasStock);
      for (let i of noStock) {
        if (!hasStock.has(i)) {
          no_stock_arr.push(i);
        }
      }
      console.log(no_stock_arr);
      this.noStockArr = no_stock_arr;
    }
  }
};
</script>
<style lang="less" scoped>
.flex {
  display: flex;
  display: -webkit-flex;
}
.page-test2 {
  @mainColor:#282323;
  .list {
    margin-top: 20px;
    background-color: #fff;
    padding: 10px;
    .list-title {
      font-weight: bold;
      font-size: 16px;
      color: #333;
    }
    .list-body {
      justify-content: flex-start;
      .value-item {
        color: #666;
        margin: 10px 10px 0 0;
        background-color: #f2f2f2;
        padding: 5px 15px;
        border-radius: 20px;
        &.active {
          color: #fff;
          background-color: @mainColor;
        }
        &.disabled {
          color: #fff;
          background-color: #dfdddb;
          text-decoration: line-through;
          pointer-events: none;
        }
      }
    }
  }
}
</style>
