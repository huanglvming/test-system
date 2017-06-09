<template>
  <div id="app">
    <div class="test-list">
      <test-item v-for="(item,index) in list" 
      :key="index" :obj="item" :index="index"
      :answer="item.answer" :identy="item.name" 
      :isSubmit="is_submit"
      ref="testItem"></test-item>
    </div>
    <div :class="['btn-submit',{'btn-disable':is_submit}]" id="submit" @click="submit()">提交</div>
    <div class="tips-wrapper" @touchmove.prevent v-show="tips" @click="hide(0)">
      <div class="tips">请先完成所有问题~</div>
    </div>
    <div class="tips-wrapper" @touchmove.prevent v-show="re" @click="hide(1)">
      <div class="tips">分数:{{result}}</div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import jquery from "jquery"
import testItem from "./components/testItem.vue"
export default {
  name: 'app',
  data(){
    return{
      list: [],
      score: 0,
      tips: false,
      re: false,
      is_submit: false,
    }
  },
  components:{
    testItem,
  },
  mounted(){
    axios('http://www.xiangwencheng.top/subject/').then(res => {
      console.log(res);
      this.list = res.data;
    }).catch(err => {
      console.log(error);
    })
  },
  computed:{
    result(){
      return this.score;
    }
  },
  methods:{
    submit(){
      let arr = this.$refs.testItem;
      for(let i=0,len=arr.length; i<len; i++){
        if(!arr[i].checked){
          this.tips = true;
          this.score = 0;
          return false;
        }else{
          this.score += arr[i].score;
        }
      }

      jquery.ajax({
        url: 'http://www.xiangwencheng.top/score/',
        method: 'post',
        data: {score: this.score.toString()},
        success:(data => {
          let obj = JSON.parse(data);
          console.log(obj);
          if(obj.ok){
            this.re = true;
            this.is_submit = true;
          }
        }),
        error: function(err){
          console.log(err);
        }
      });
      // axios({
      //   url: 'http://www.xiangwencheng.top/score/',
      //   method: 'post',
      //   headers: {
      //     'Content-Type': 'application/x-www-form-urlencoded'
      //   },
      //   data: {
      //     score: '80'
      //   },
      //   responseType: 'json',
      // }).then(res => {
      //   console.log(res);
      // }).catch(err => {
      //   console.log("xxxxxxxxxx");
      // });
    },
    hide(type){
      if(type === 0){
        this.tips = false;
      }else if(type === 1){
        this.re = false;
        this.score = 0;
      }
    },
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 10px;
  font-size: 12px;
  text-align: left;
}

.btn-submit{
  display: inline-block;
  position: relative;
  left: calc(50% - 1rem);
  width: 2rem;
  height: 0.8rem;
  margin-top: 1rem;
  text-align: center;
  line-height: 0.8rem;
  border-radius: 5px;
  background-color: #41B883;
  color: white;
  cursor: pointer; 
}
.btn-disable{
  pointer-events: none;
  background-color: #dedede;
  color: #333;
}
.tips-wrapper{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.4);
}
.tips{
  position: absolute;
  top: calc(50% - 1rem);
  left: calc(50% - 2rem);
  width: 4rem;
  height: 2rem;
  line-height: 2rem;
  text-align: center;
  border-radius: 0.2rem;
  background-color: white;
}
</style>
