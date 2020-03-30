<template>
  <div id="app" >
    <Header 
      v-bind:correct="correct"
      v-bind:total="total"
    />
    <b-container class="bv-example-row" >
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
          v-if="questions.length" 
          v-bind:ques="questions[index]"
          v-bind:next="next"
          v-bind:increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import QuestionBox from './components/QustionBox.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    QuestionBox,
    Header
  },
  data(){
    return {
    questions:[],
    index:0,
    correct:0,
    total:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.correct++
      }
      this.total++
    }
  }
  ,
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
      method:'get'
    })
    .then((response) => {
      return response.json()
    })
    .then((jsondata) =>{
      this.questions=jsondata.results
    })
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
  margin-top: 60px;
}
</style>
