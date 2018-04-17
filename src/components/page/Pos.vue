<template>
  <div>
    <el-row>
      <el-col class="pos-order" :span='9'>
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table border :data="tableData">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column  label="操作">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total-div">
              <small>数量：</small>{{totalCount}} &nbsp;&nbsp;&nbsp;<small>金额：</small>{{totalMoney}} 元
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="checkout">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="15" class="right-div">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="(goods,index) in oftenGoods" :key="index" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                    <li v-for="(goods,index) in type0Goods" :key="index" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <div class="detail-div">
                          <span class="foodName">{{goods.goodsName}}</span>
                          <span class="foodPrice">￥{{goods.price}}元</span>
                        </div>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class='cookList'>
                    <li v-for="(goods,index) in type1Goods" :key="index" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <div class="detail-div">
                          <span class="foodName">{{goods.goodsName}}</span>
                          <span class="foodPrice">￥{{goods.price}}元</span>
                        </div>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class='cookList'>
                    <li v-for="(goods,index) in type2Goods" :key="index" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <div class="detail-div">
                          <span class="foodName">{{goods.goodsName}}</span>
                          <span class="foodPrice">￥{{goods.price}}元</span>
                        </div>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class='cookList'>
                    <li v-for="(goods,index) in type3Goods" :key="index" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <div class="detail-div">
                          <span class="foodName">{{goods.goodsName}}</span>
                          <span class="foodPrice">￥{{goods.price}}元</span>
                        </div>
                    </li>
                </ul>
              </div>
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
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    };
  },
  created() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        // console.log(response);
        this.oftenGoods = response.data;
      })
      .catch(error => {
        // console.log(error);
        this.$message.error("网络错误，不能访问");
      });

    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        // console.log(response);
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        // console.log(error);
        this.$message.error("网络错误，不能访问");
      });
  },
  methods: {
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      if (isHave) {
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        // console.log(arr);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },

    // 删除单个商品
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.getAllMoney();
    },

    // 删除所有商品
    delAllGoods() {
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },

    // 数量和金额
    getAllMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        // 计算金额和数量
        this.tableData.forEach(element => {
          // console.log(element);
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },

    // 模拟结账
    checkout() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
        this.$message({
          message:'结账成功，感谢您又为该店出了一份力',
          type:'success'
        });
      } else {
        this.$message.error('不能空结，老板了解您急切的心情！');
      }
    }
  }
};
</script>


<style scoped>
.right-div {
  background-color: #f7f7f7;
  padding-bottom: 30px;
}
.pos-order {
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul {
  display: flex;
  flex-wrap: wrap;
}
.often-goods-list ul li {
  list-style: none;
  border-block-end: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #5887ff;
}
.cookList {
  display: flex;
  flex-wrap: wrap;
  -webkit-margin-before: 0;
  -webkit-margin-after: 0;
}
.cookList li {
  list-style: none;
  border: 1px solid #e5e9f2;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  margin: 2px;
  display: flex;
  cursor: pointer;
}
.cookList .detail-div {
  display: flex;
  flex-direction: column;
  padding-left: 10px;
  width: 100px;
  justify-content: center;
  align-items: flex-start;
}
.foodImg {
  width: 60px;
}
.foodName {
  font-size: 16px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-top: 10px;
}
.total-div {
  background: #fff;
  padding: 10px;
  border-bottom: 1px solid #ebeef5;
}
</style>
