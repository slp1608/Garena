<template>
  <div class="hello">
    <div v-if="isHorizontalScreen">
      <div class="main">
        <div class="header">
          <img src="../assets/bg.png" alt="">
        </div>
        <div class="subHeader">
          <div class="line"></div>
          <div class="subHeaderContent">
            <div class="left"></div>
            CHARACTER
            <div class="right"></div>
          </div>
        </div>
        <div class="content">
          <div class="characterItem" v-for="item in imgList" :key="item.name">
            <img :src="item.path" :alt="item.name">
          </div>
        </div>
        <!-- <img class="pcImg" src="../assets/PC.jpg" alt="">
        <img class="padImg" src="../assets/ipad.jpg" alt="">
        <img class="moblieImg" src="../assets/../assets/moblie.png" alt=""> -->
      </div>
    </div>
    <div v-else>
      <div class="verticalScreenPage">
        <img class="verticalScreenImg" alt="scape" src="../assets/scape.png">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return {
      imgList: []
    }
  },
  props: {
    isHorizontalScreen: {
      type: Boolean,
      default: false,
    }
  },
  mounted() {
    this.renderResize()
    // 监听resize方法
    window.addEventListener("resize", this.renderResize, false);
    this.getData()
  },
   methods: {
    renderResize() {
      let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
      // 判断是否为手机端
      if(flag){
        let width = document.documentElement.clientWidth;
        let height = document.documentElement.clientHeight;
        if (width > height) {
          this.isHorizontalScreen = true;
          this.isVerticalScreen = false;
        } else {
          this.isHorizontalScreen = false;
          this.isVerticalScreen = true;
        }
      } else {
        this.isHorizontalScreen = true;
      }
    },
    getData() {
      // var xhr = new XMLHttpRequest();
      // xhr.open('GET', url/file,true);
      // xhr.onreadystatechange = function() {
      //   if(xhr.readyState==4){
      //         if(xhr.status==200){
      //             var data=xhr.responseText;
      //             console.log(data);
      //   }
      // };
      // xhr.onerror = function() {
      //   console.log("Oh, error");
      // };
      // xhr.send();
      let that = this;
      fetch("./demoData.json").then(response => response.json())
      .then(function(data){
          console.log(data)
          that.imgList = data.data
        })
      .catch(err => console.log("Oh, error", err))
    }
  }
}
</script>

<style scoped>
.subHeader{
  position: relative;
  text-align: center;
  color: #fff;
  padding-top: 60px;
}
.subHeader .line{
  position: absolute;
  top: 83px;
  width: 100%;
  height: 1px;
  background-color: #111518;
}
.subHeaderContent{
  position: relative;
  z-index: 9;
  display: inline-block;
  padding: 14px 20px;
  border: 1px solid #30353b;
  background-color: #111518;
}
.main{
  background-color: #1e2327;
}
.main img{
  width: 100%;
}
.content{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 80%;
  margin: 0 auto;
  padding: 60px 0;
}
/*moblie*/
@media screen and (max-width:600px){
  /* .moblieImg{
    display: initial;
  }
  .pcImg, .padImg{
    display: none;
  } */
  .characterItem{
    width: 80%;
  }
}
/*pad*/
@media screen and (min-width:600px) and (max-width:960px){
  /* .padImg{
    display: initial;
  }
  .pcImg, .moblieImg{
    display: none;
  } */
  .characterItem{
    width: 40%;
  }
}
/*PC*/
@media screen and (min-width:960px){
  /* .pcImg{
    display: initial;
  }
  .moblieImg, .padImg{
    display: none;
  } */
  .characterItem{
    width: 30%;
  }
}
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(-90deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
.verticalScreenPage{
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  background-color: rgba(0,0,0,.5);
}
.verticalScreenImg{
  width: 100%;
  animation: spin 1s linear infinite;
}
.characterItem{
    margin-bottom: 30px;
  }

</style>
