<template>
	<div class="test-item">
      <p class="title">{{index+1}}.{{obj.title}}</p>
      <ul class="choice-list">
        <li v-for="(item,index) in obj.choice" :class="{correct: index == (answer-1) && isSubmit}">
          <input type="radio" :name="identy" :disabled="isSubmit" :value="index == answer-1" @click="checkAnswer">
          {{index | Char(index)}}.{{item}}
        </li>
      </ul>
    </div>
</template>

<script>
	export default{
		name: 'testItem',
		props: ['obj','index','answer',"identy","isSubmit"],
		data(){
			return {
				score: 0,
				checked: false,
			}
		},
		filters:{
			Char(value){
				return String.fromCharCode(value + 65);
			}
		},
		methods:{
			checkAnswer(event){
				this.checked = true;
				if(event.currentTarget.value == 'true'){
					this.score += 10;
				}else if(this.score>0){
					this.score -= 10;
				}
				console.log('score:'+this.score);
			}
		}
	}	
</script>

<style lang="scss" scoped>
.title{
  margin-bottom: 5px;
  font-size: 14px;
  font-weight: bold;
}
.choice-list{
  display: flex;
  flex-wrap: wrap;
  li{
    min-width: 50%;
    max-width: 100%;
  }
}
.test-item:not(:first-child){
  margin-top: 0.5rem;
}
.correct{
	color: green;
}
</style>