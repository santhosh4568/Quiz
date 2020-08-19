<template>
    <div class= "questionbox-container">
  <b-jumbotron>
    

    <template v-slot:lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">
     <b-list-group>
        <b-list-group-item 
          v-for="(answer,index) in shuffledAnswers" 
          :key="index" 
          @click="selectedanswer(index)"
          :class="[
            !answered && selectedindex === index ?'selected':
            answered && correctIndex===index?'correct':
            answered && selectedindex===index && correctIndex !==index?'incorrect':''
            ]"
          >
            {{answer}}
            
        </b-list-group-item>
    
    </b-list-group>
    

    <b-button variant="primary" @click="submitAnswer" :disabled="selectedindex===null || answered">
        Submit</b-button>
    <b-button  @click="next" variant="success" href="#">Next Question</b-button>
  </b-jumbotron>
</div>
</template>

<script>

import _ from 'lodash'
export default {
    props:{
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return{
            selectedindex :null,
            shuffledAnswers:[],
            correctIndex: null,
            answered:false
        }
    },
    computed:{
        answers(){
            let answers=[...this.currentQuestion.incorrect_answers]
             answers.push(this.currentQuestion.correct_answer)
             return answers        
        }
    },
    watch:
    {
        currentQuestion:
        {
            immediate:true,
            handler(){
                this.selectedindex=null
                this.shuffleAnswers()
                this.answered=false
            }
        }
    },
    
    methods :
    {
        selectedanswer(index)
        {
            this.selectedindex=index
        },
        submitAnswer()
        {
            let isCorrect=false
            if(this.selectedindex === this.correctIndex)
            {
                isCorrect=true
            }
            this.answered=true
            this.increment(isCorrect)
            
        },
        shuffleAnswers()
        {
            let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.shuffledAnswers= _.shuffle(answers)
            this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }
    }
}
</script>

<style scoped>
 .list-group{
     margin-bottom:15px;
 }
 .list-group-item:hover{
     background-color: #EEE;
 }
 .btn{
     margin: 0 5px
 }
 .selected{
     background-color:aqua;
 }
 .correct{
     background-color: green;
 }
 .incorrect
 {
     background-color: red;
 }
</style>