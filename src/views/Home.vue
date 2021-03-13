<template>
  <div class="home container">
  
    <h1>Test your knowledge</h1>
    <br>
    <button class="btn btn-success" v-on:click="start">Start</button>


    <br><br>
    <hr>

    <div class="card" v-for="(question,index) in questions" :key="question.questions">
    {{question.question}}

    <button class="btn btn-secondary m-1" v-for="answer in question.incorrect_answers " :key="answer" v-on:click="addAnswer(answer,index)">{{answer}}</button>

    <button class="btn btn-secondary m-1" v-on:click="addAnswer(question.correct_answer,index)">{{question.correct_answer}}</button>

    <p>{{question.message}}</p>

    </div>

    <p>current score is {{score}}</p>

    
   
  </div>

  



 



</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  data(){
  return {
  questions : [],
  answers : [],
  score : 0
  }
  },
  methods: {

  shuffleArray:function(array) {
  var currentIndex = array.length, temporaryValue, randomIndex;

  // While there remain elements to shuffle...
  while (0 !== currentIndex) {

    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;

    // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }

  return array;
},


  addAnswer:function(answer, index){
  if(this.questions[index].correct_answer == answer){
  this.score ++
  this.questions[index].message = "you answer correctly"
  }
  else {
  this.questions[index].message = "your answer is incorrect";
  }
  console.log("current score is"+this.score )
  },
  start:function(){
  fetch('https://opentdb.com/api.php?amount=10&category=17&type=multiple')
  .then(response => response.json())
  .then(data => {
  this.questions = data["results"] 
  for (var i = 0; i< this.questions.length; i++){
  var options = this.questions[i].incorrect_answers
  options.push(this.question[i].correct_answer)
  var shuffledOptions = this.shuffleArray(options);
  this.questions[i].options = shuffledOptions;
  }

   
}) 
}
}
}
</script>
