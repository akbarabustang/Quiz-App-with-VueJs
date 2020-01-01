<template>
  <div>
    <div>
      <b-nav tabs>
        <b-nav-item disabled><b>Quiz App</b></b-nav-item>
        <b-nav-item disabled>Correct : {{ numCorrect }}</b-nav-item>
        <b-nav-item disabled>Answered : {{ numTotal }}</b-nav-item>
        <b-nav-item disabled>Question : {{ numQuestion }}</b-nav-item>
      </b-nav>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "numCorrect",
    "numTotal",
    "numQuestion"
  ]
}
</script>