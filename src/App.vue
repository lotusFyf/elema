<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to ='/goods' :class="seller">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to = '/rating'>评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to ='/seller'>商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
 import header from './components/header/header.vue'

 export default {
  data(){
    return{
      seller:{}
    }
  },
  created(){
      var _this = this;
      _this.$http.get('../static/data.json').then(function(response){
      _this.seller = response.data.seller
    })
  },
  components:{
    'v-header':header
  }
 }


</script>

<style>
#app .tab{
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid rgba(7,17,27,0.1)
}
#app .tab .tab-item{
  flex: 1;
  text-align: center;
}
#app .tab .tab-item a{
  display: block;
  font-size: 14px;
  color: rgb(77, 85, 93)
}
#app .tab .tab-item .active{
  color: rgb(240, 20, 20);
}
</style>
