<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-31goumaichongzhi"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-31huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-tongji"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Welcome to my Pos example'
    }
  }
}
</script>

<style>
.left-nav{
  color: #fff;
  font-size: 10px;
  background-color: #1D8CE0;
  width: 56px;
  position: fixed;
  top: 0;
  left: 0;
  bottom:0;
}
.iconfont{
  font-size: 24px;
}
.left-nav ul{
  padding: 0;
  margin: 0;
}
.left-nav li{
  list-style: none;
  text-align: center;
  border-bottom: 1px solid #20a0ff;
  padding: 10px;
}
</style>
