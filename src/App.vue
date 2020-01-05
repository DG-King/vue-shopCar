<template>
  <div id="app">
    <header>购物车</header>
    <div class="container">
      <div v-if="shopCar.length > 0">
        <div class="item" v-for="(item, index) in shopCar" :key="index">
          <mt-cell-swipe
            :right="[
              {
                content: '删除',
                style: { background: 'red', color: '#fff',display: 'flex', alignItems: 'center' },
                handler: () => del(index)
              }
            ]">
              <div style="flex: 1">
                <div class="logo">
                  <img :src="item.businessLogo" alt="">
                  <span>{{ item.businessName }}</span>
                </div>
                <div class="details">
                  <img :src="item.commodityImg" alt="">
                  <div class="details-list">
                    <span>{{ item.commodityName }}</span>
                    <div class="list-more">
                      <span>颜色: {{ item.commodityColor }}</span>
                      <span>尺码: {{ item.commoditySize }}</span>
                    </div>
                    <div class="list-price">￥{{ item.commodityPrice }}/件</div>
                  </div>
                </div>
                <div class="num">
                  <span>购买数量</span>
                  <div>
                    <button @click="btn(false, index)">-</button>
                    <input type="text" :value="item.num" @blur="blurInput($event, index)"/>
                    <button @click="btn(true, index)">+</button>
                  </div>
                </div>
              </div>
          </mt-cell-swipe>
        </div>
      </div>
      <div v-else class="empty">暂无数据</div>
    </div>
    <footer>
      <div class="footer-left">
        实际付款:
        <span>￥{{ totalPrice }}免运费</span>
      </div>
      <div class="footer-right">立即付款</div>
    </footer>
  </div>
</template>

<script>
import Vue from 'vue'
import { CellSwipe } from 'mint-ui';
Vue.component(CellSwipe.name, CellSwipe);
export default {
  data() {
    return {
      totalPrice: 0,
      shopCar: [
        {
          id: 1,
          businessLogo: 'https://b-gold-cdn.xitu.io/v3/static/img/simplify-logo.3e3c253.svg',
          businessName: '商家名称111',
          commodityImg: 'https://user-gold-cdn.xitu.io/2019/5/23/16ae258dd9521486?imageView2/1/w/100/h/100/q/85/format/webp/interlace/1',
          commodityName: '商品名称111',
          commodityColor: '墨绿色',
          commoditySize: 'L11-202',
          commodityPrice: 9.9,
          num: 1
        },
        {
          id: 2,
          businessLogo: 'https://b-gold-cdn.xitu.io/v3/static/img/simplify-logo.3e3c253.svg',
          businessName: '商家名称222',
          commodityImg: 'https://user-gold-cdn.xitu.io/2019/5/23/16ae258dd9521486?imageView2/1/w/100/h/100/q/85/format/webp/interlace/1',
          commodityName: '商品名称222',
          commodityColor: '墨绿色2',
          commoditySize: 'L11-202',
          commodityPrice: 29.9,
          num: 1
        },
        {
          id: 3,
          businessLogo: 'https://b-gold-cdn.xitu.io/v3/static/img/simplify-logo.3e3c253.svg',
          businessName: '商家名称333',
          commodityImg: 'https://user-gold-cdn.xitu.io/2019/5/23/16ae258dd9521486?imageView2/1/w/100/h/100/q/85/format/webp/interlace/1',
          commodityName: '商品名称333',
          commodityColor: '墨绿色3',
          commoditySize: 'L11-202',
          commodityPrice: 39.9,
          num: 1
        },
        {
          id: 4,
          businessLogo: 'https://b-gold-cdn.xitu.io/v3/static/img/simplify-logo.3e3c253.svg',
          businessName: '商家名称444',
          commodityImg: 'https://user-gold-cdn.xitu.io/2019/5/23/16ae258dd9521486?imageView2/1/w/100/h/100/q/85/format/webp/interlace/1',
          commodityName: '商品名称444',
          commodityColor: '墨绿色4',
          commoditySize: 'L11-202',
          commodityPrice: 49.9,
          num: 1
        },
        {
          id: 5,
          businessLogo: 'https://b-gold-cdn.xitu.io/v3/static/img/simplify-logo.3e3c253.svg',
          businessName: '商家名称555',
          commodityImg: 'https://user-gold-cdn.xitu.io/2019/5/23/16ae258dd9521486?imageView2/1/w/100/h/100/q/85/format/webp/interlace/1',
          commodityName: '商品名称555',
          commodityColor: '墨绿色5',
          commoditySize: 'L11-202',
          commodityPrice: 59.9,
          num: 1
        }
      ]
    }
  },
  methods: {
    getTotalPrice() {
      let price = 0;
      this.shopCar.map(item =>
      {
        price += item.num * item.commodityPrice
      });
      this.totalPrice = price.toFixed(2);
    },
    btn(bool, index) {
      let shopIndex = this.shopCar[index];
      if(bool) {
        shopIndex.num ++;
      }
      else {
        if(shopIndex.num <= 1) {
          return;
        }
        shopIndex.num --;
      }
      this.getTotalPrice();
    },
    blurInput(event, index) {
      this.shopCar[index].num = event.target.value < 1 ? 1 : Number(event.target.value);
      this.getTotalPrice();
    },
    del(index) {
      this.shopCar.splice(index, 1);
      this.getTotalPrice();
    }
  },
  mounted() {
    this.getTotalPrice();
  },
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
    background: #f5f5f5;
  }
  header {
    height: 50px;
    width: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 99;
    border-bottom: 1px solid #ccc;
    text-align: center;
    line-height: 50px;
    background: #fff;
  }
  .container {
    background: #fff;
    padding-top: 50px;
    margin-bottom: 50px;
  }
  .container>.item + .item {
    border-top: 1px solid #ccc;
  }
  .logo {
    display: flex;
    align-items: center;
    height: 40px;
  }
  .logo img {
    width: 20px;
    height: 20px;
  }
  .logo span {
    color: #888;
    font-size: 12px;
    margin-left: 20px;
  }
  .details {
    background: #f5f5f5;
    padding: 15px;
    display: flex;
    font-size: 12px;
  }
  .details img {
    width: 90px;
    height: 90px;
  }
  .details-list {
    margin-left: 15px;
    color: #888;
    line-height: 16px;
  }
  .list-more span {
    display: block;
    color: #ccc;
  }
  .list-price {
    font-size: 14px;
  }
  .num {
    color: #888;
    padding: 10px;
    display: flex;
    justify-content: space-between;
  }
  .num button {
    width: 31px;
    height: 25px;
    background: #e0e0e0;
    color: #58595b;
    border: none;
    outline: none;
  }
  .num input {
    width: 37px;
    height: 25px;
    border: none;
    text-align: center;
    background: #fff;
  }
  footer {
    display: flex;
    width: 100%;
    height: 50px;
    position: fixed;
    bottom: 0;
    left: 0;
    background: #fff;
    border-top: 1px solid #ccc;
    z-index: 98;
  }
  .footer-left {
    flex: 1;
    text-align: center;
    line-height: 50px;
    font-size: 16px;
  }
  .footer-left span {
    color: red;
  }
  .footer-right {
    background: red;
    color: #fff;
    width: 150px;
    height: 50px;
    line-height: 50px;
    text-align: center;
  }
  .empty {
    text-align: center;
    height: calc(100vh - 100px);
    line-height: calc(100vh - 100px);
    background: #f5f5f5;
  }
  .mint-cell-title {
    display: none;
  }
  .mint-cell-value {
    flex: 1;
  }
</style>
