<template>
    <div class="leftNav">
      <ul>
          <li><i class="icon iconfont icon-goumai"></i><span>收银</span></li>
          <li><i class="icon iconfont icon-dianpu"></i><span>店铺</span></li>
          <li><i class="icon iconfont icon-hanbao"></i><span>商品</span></li>
          <li><i class="icon iconfont icon-huiyuanqia"></i><span>会员</span></li>
          <li><i class="icon iconfont icon-tongji"></i><span>统计</span></li>
          <li><i class="icon iconfont icon-gongnengjianyi"></i><span>设置</span></li>
      </ul>
      </div> 
</template>
<script>
export default {
  name:"leftNav",
  data(){
      return{}
  }
}
</script>
<style>
.leftNav{
   height: 100%;
   width: 8%;
   background-color: #1D8ce0;
   font-size: 14px;
   color: #ffffff; 
}
.leftNav ul{
    margin: 0;
    padding: 0;
    display: flex;
    display: -webkit-flex;
    list-style: none;
    flex-direction: column;
    justify-content: flex-start;
}
.leftNav li{
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
    list-style: none;
    display: flex;
    display: -webkit-flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;

    
}
.leftNav li span{
    display: block;
}
.leftNav .iconfont{
    font-size: 20px;
}
</style>

