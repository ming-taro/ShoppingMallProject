<template>
  <div>
    <div id="wrap">
      <HeaderPart/>
      <router-view></router-view>
      <button class="top-btn" >
        <a href="#">
          <img src="http://ec2-13-125-74-101.ap-northeast-2.compute.amazonaws.com:3000/images/topButton.png" width="50">
        </a>
      </button>
      <FooterPart/>
    </div>
  </div>
</template> 

<script>
import HeaderPart from './components/HeaderPart.vue';
import FooterPart from './components/FooterPart.vue';

export default {
  name: 'App',
  data(){  //데이터 보관함
    return {
    }
  },
  methods :{
  },
  components: {
    HeaderPart,
    FooterPart,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.top-btn {
  position: fixed;
  bottom: 10%;
  right: 2%;
  border: 0;
  background-color:transparent;
}
</style>