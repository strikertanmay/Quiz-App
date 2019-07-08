<template>
<div class="question-box-container">
  <b-jumbotron  lead="Bootstrap component for vue.js"> 
        <template slot="lead">
          {{currentQues.question | decoder }}
        </template>
        <hr class="my-4">
       

       <div >
            <b-list-group>
                <b-list-group-item button 
                v-for="(answer,index) in shuffeledAnswers" 
                :key="index"
                @click.prevent="selectAnswer(index)"
                :class="answerClass(index)"
                >
                    {{ answer | decoder }}
                </b-list-group-item>
            </b-list-group>
       </div>
        

        <b-button 
        variant="primary"
        @click = "submitAnswer"
        :disabled = "selectedIndex===null || answered"
        >
        Submit
        </b-button>
        <b-button 
        @click="next"
        :disabled = "lastQ" 
        variant="success" 
        >Next
        </b-button>
  </b-jumbotron>
</div>
</template>
<script>

import _ from 'lodash';

export default {
    props: {
        currentQues : Object,
        next: Function,
        increment : Function,
        lastQ :  Boolean,
        
    },
    data(){
        return{
            selectedIndex: null,
            correctIndex:null,
            shuffeledAnswers: [],
            answered: false,
        }
    },
    computed: {
        answers(){
            let answers = [...this.currentQues.incorrect_answers]
            answers.push(this.currentQues.correct_answer)
            return answers
        }

    },
    watch:{
        currentQues: {
            immediate:true,
            handler(){
                this.selectedIndex = null
                this.answered = false
                this.shuffelAnswers()
            }
            
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffelAnswers() {
            let answers = [...this.currentQues.incorrect_answers, this.currentQues.correct_answer]
            this.shuffeledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffeledAnswers.indexOf(this.currentQues.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true   
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index) {
            
            let answerClass = ''
            if(!this.answered && this.selectedIndex===index){
                answerClass = 'selected'
            }
            else if(this.answered && this.correctIndex ===index ){
                answerClass =  'correct'
            }
            else if(this.answered && this.selectedIndex===index && this.correctIndex !== index ){
                answerClass = 'incorrect'
            }

            return answerClass; 
        }
    },
    filters: {
    decoder(str) {
      var textArea = document.createElement("textarea");
      textArea.innerHTML = str;
      return textArea.value;
    }
  }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px; 
}
.list-group-item:hover{
     background: #eeeeee;
     cursor: pointer;
}
.btn{
    margin: 0 5px;
}

.selected{
    background-color: lightblue;
}

.correct{
    background-color: lightgreen;
}

.incorrect{
    background-color: red;
}
</style>
