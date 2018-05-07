<template>
  <div class="container">
      <el-row class="content">
        <el-col :span="8" class="book_list">
          <el-tabs>
            <el-tab-pane label="点餐" class="overscroll">
              <el-table border style="width: 100%" :data="tableData">
                <el-table-column label="商品名称" prop="goodsName" max-width="150">

                </el-table-column>
                <el-table-column label="数量" prop="count" width="50">

                </el-table-column>
                <el-table-column label="金额" prop="price" width="70">

                </el-table-column>
                <el-table-column label="操作" prop="operate" fixed="right" min-width="150">
                    <template slot-scope="scope">
                      <el-button type="primary" size="mini"  plain @click="addOrder(scope.row)">增加</el-button>
                      <el-button type="danger" size="mini"  plain @click="delSingleGoods(scope.row)">删除</el-button>
                    </template>
                </el-table-column>
              </el-table>
              <div class="total">
                <span>数量：</span>{{totalCount}}<span>金额：</span>{{totalMoney}}元
              </div>
              <div class="buttom_btns">
                <el-button type="warning" @click="" size="small">挂单</el-button>
                <el-button type="danger" @click="delAllGoods"  size="small">删除</el-button>
                <el-button type="success" @click="checkout"  size="small">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单"  class="overscroll" >
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖"  class="overscroll">
              外卖
            </el-tab-pane>
          </el-tabs>
          
        </el-col>
        <el-col :span="16" class="goods_list">
          <div class="hot_goods">
              <div class="title">
                热门商品
              </div>
              <ul class="hot_list">
                <li v-for='goods in hotGoods' @click="addOrder(goods)">
                  <span>{{goods.goodsName}}</span>
                  <span class="hot_price">¥{{goods.price}}元</span>
                </li>
                
              </ul>
          </div>
          <!---->
          <div class="goods_type_container">
            <el-tabs type="border-card">
               <el-tab-pane label="汉堡" class="hanbuger">
                <ul>
                  <li v-for='goods in type0Goods' @click="addOrder(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div>
                      <span>{{goods.goodsName}}</span>
                      <span class="price">¥{{goods.price}}元</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
               <el-tab-pane label="小吃" class="hanbuger">
                <ul>
                  <li v-for='goods in type1Goods' @click="addOrder(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div>
                      <span>{{goods.goodsName}}</span>
                      <span class="price">¥{{goods.price}}元</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
               <el-tab-pane label="饮料" class="hanbuger">
                <ul>
                  <li v-for='goods in type2Goods' @click="addOrder(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div>
                      <span>{{goods.goodsName}}</span>
                      <span class="price">¥{{goods.price}}元</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
               <el-tab-pane label="套餐" class="hanbuger">
                <ul>
                  <li v-for='goods in type3Goods' @click="addOrder(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div>
                      <span>{{goods.goodsName}}</span>
                      <span class="price">¥{{goods.price}}元</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
            
          </div>
        </el-col>
      </el-row>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      tableData: [
        /* {
          goodsName: "可口可乐",
          price: 8,
          count: 1
        }*/
      ],
      hotGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalCount: 0,
      totalMoney: 0,
      totalPrice: 0
    };
  },
  created() {
    // axios.get('http://jspang.com/DemoApi/oftenGoods.php').then(res=>{
    //   this.hotGoods = res.data;
    // }).catch(e=>{
    //   alert("网络错误，请重试！")
    // });
    // axios.get('http://jspang.com/DemoApi/typeGoods.php').then(res=>{
    //   this.type0Goods = res.data[0];
    //   this.type1Goods = res.data[1];
    //   this.type2Goods = res.data[2];
    //   this.type3Goods = res.data[3];
    // }).catch(e=>{
    //   alert("网络错误，请重试！")
    // });
    //axios.get(url,params:{id:""})
    //axios.post(url,{id:""})

    //两个请求都完成后触发；
    axios
      .all([
        axios.get("http://jspang.com/DemoApi/oftenGoods.php"),
        axios.get("http://jspang.com/DemoApi/typeGoods.php")
      ])
      .then(
        axios.spread((res1, res) => {
          this.hotGoods = res1.data;
          this.type0Goods = res.data[0];
          this.type1Goods = res.data[1];
          this.type2Goods = res.data[2];
          this.type3Goods = res.data[3];
        })
      );
  },
  methods: {
    addOrder(goods) {
      let isExist = false;

      this.tableData.find(item => {
        if (item.goodsId == goods.goodsId) {
          isExist = true;
        }
      });
      //当前商品已经在列表
      if (isExist) {
        this.tableData.forEach(item => {
          if (item.goodsId == goods.goodsId) {
            item.count++;
          }
        });
      } else {
        //添加新的商品
        let newGoods = {
          goodsName: goods.goodsName,
          goodsId: goods.goodsId,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.totalChange();
    },
    totalChange() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData.length) {
        this.tableData.forEach(item => {
          this.totalCount += item.count;
          this.totalMoney += item.count * item.price;
        });
      }
    },
    delSingleGoods(goods) {
      this.tableData.forEach((item, i) => {
        if (item.goodsId === goods.goodsId) {
          if (item.count > 1) {
            item.count--;
            console.log(this);
          } else {
            this.tableData.splice(i, 1);
          }
          this.totalChange();
        }
      });
    },
    delAllGoods() {
      this.tableData = [];
      this.totalChange();
    },
    checkout(params){
      if(this.tableData.length){
      this.delAllGoods();
      this.$message({
        message:"结账完成，继续加油",
        type:'success'
      })
      }else{
        this.$message.error("暂无商品！")
      }

    
    }
  }
};
</script>
<style scoped>
.container {
  width: 95%;
  float: left;
  height: 100%;
}
.content {
  height: 100%;
  background-color: #f8f8f8;
}
.overscroll {
  overflow: auto;
}
.book_list {
  height: 100%;
  background-color: #f8f8f8;
  border-right: 1px solid #e7e7e7;
}
.el-table-column {
  text-align: center;
}
.buttom_btns {
  margin-top: 30px;
}
.goods_list {
}
.hot_list {
  max-height: 300px;
  overflow: auto;
}
.hot_goods .title {
  height: 34px;
  line-height: 34px;
  text-align: left;
  padding-left: 15px;
  border-bottom: 1px solid #e7e7e7;
  background-color: #ffffff;
}
.hot_goods .hot_list {
  list-style: none;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  align-self: center;
}
.hot_list li {
  border: 1px solid #ffffff;
  background-color: #ffffff;
  padding: 5px 10px;
  margin-bottom: 15px;
}
.hot_price {
  color: #1d8ce0;
}
.hanbuger ul {
  list-style: none;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.hanbuger li {
  margin-bottom: 15px;
  //padding: 0 10px;
  display: flex;
}
.hanbuger li > div {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  margin-left: 15px;
}
.hanbuger .price {
  color: #1d8ce0;
  margin-top: 20px;
}
.hanbuger li img {
  height: 80px;
  width: 80px;
}
.total {
  width: 100%;
  text-align: right;
  margin-top: 20px;
}
.total span {
  margin-left: 20px;
}
</style>



