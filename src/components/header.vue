<template>
<div>
  <b-jumbotron header="Quiz App" >
    <h4>Correct: {{numCorrect}}</h4 >
    <h4>Attempt : {{numTotal}} </h4>
  </b-jumbotron>
</div>

 
</template>

<script>

export default{
    props:[
        'numCorrect',
        'numTotal' 
    ]
}
</script>