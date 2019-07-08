<template>
  <div id="app">
    <Header
    
    :numCorrect = "numCorrect"
    :numTotal = "numTotal"

     />


<b-container class="bv-example-row" style="margin-top:3rem;">
     <div class="text-center" v-if="!qs" style="margin-top:2rem;">
           <b-spinner  style="width: 4rem; height: 4rem;"  label="Loading..."></b-spinner>  
      </div>
  <b-row v-if="qs"> 
    <b-col sm="6" md="8" offset-xl="2" offset-md="2" >
      <Questionbox 
        v-if="ques.length"
        :currentQues="ques[index]"
        :next = "next"
        :increment="increment"
        :lastQ = "index+1===TotalQ"
        :qs = "qs"
      />
    </b-col>
  </b-row>
</b-container>
    
  </div>
</template>

<script>

import Header from './components/header.vue';
import Questionbox from './components/questionbox.vue';
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Header,
    Questionbox
  },
  data() {
    return {
      ques: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      TotalQ: 10,
      qs : false
    } 
  },
  methods : {
    next(){
    
      if(this.index < this.TotalQ){
        this.index++
      }
    },
    increment(isCorrect){
            if(isCorrect){
                this.numCorrect++
            }
            this.numTotal++
        }
  },
  mounted : function() {
    axios.get('https://opentdb.com/api.php?amount=10&category=18&difficulty=easy&type=multiple').then((res) => {
      // console.log(res.data.results);
      this.ques =  res.data.results
      this.qs = true
    })
    .catch((err) => {
        console.log(err);
    })
    
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
