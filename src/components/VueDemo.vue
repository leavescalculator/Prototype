<template>
  <div class="hello">
    <button
        v-if="previousQuestions.length !== 0"
        v-on:click="goBack"
    >Previous Question</button>
    <!-- banked hours -->
    <div>banked hours: {{bankedHours}}</div>
    <div v-if="stateType === 'endpoint'">
        <a :href="jsonData.endpoints[currentState].url">
            <h3>{{jsonData.endpoints[currentState].title}}</h3>
        </a>
    </div>
    <div v-else-if="stateType === 'question'">
        <h3>{{jsonData.questions[currentState].title}}</h3>
        <p>{{jsonData.questions[currentState].description}}</p>
        <div v-for="(answer, index) in jsonData.questions[currentState].answers" v-bind:key="answer">
            <button v-on:click="selectAnswer(answer, index)">
                {{ answer.title }}
            </button>
            <p>{{ answer.description }}</p>
        </div>
    </div>
    <div v-else-if="stateType === 'report'">
        <table align="center">
            <tr>
                <th>Question</th>
                <th>Is Paid</th>
                <th>Added Hours</th>
            </tr>
            <tr v-for="previousQuestion in previousQuestions" v-bind:key="previousQuestion">
                <td>{{jsonData.questions[previousQuestion.questionIndex].title}}</td>
                <td>{{jsonData.questions[previousQuestion.questionIndex].answers[previousQuestion.answerIndex].isPaid}}</td>
                <td>{{jsonData.questions[previousQuestion.questionIndex].answers[previousQuestion.answerIndex].addedHours}}</td>
            </tr>
        </table>
    </div>
    <div v-else>
        Unknown state.
    </div>
  </div>
</template>

<script>
import json from '@/static/data.json'
export default {
  name: 'VueDemo',
  data() {
        return {
            currentState: 0,
            stateType: 'question',
            jsonData: json,
            bankedHours: 0,
            previousQuestions: []
        }
  },
  methods: {
    selectAnswer: function(answer, answerIndex) {
        this.previousQuestions.push({
            questionIndex: this.currentState,
            answerIndex: answerIndex
        })
        this.stateType = answer.nextState.type
        this.bankedHours += answer.addedHours
        if(this.stateType === 'question') {
            this.currentState = answer.nextState.index
        } else if(this.stateType === 'endpoint') {
            this.currentState = answer.nextState.index
        }
    },
    goBack: function() {
        var previousQuestion = this.previousQuestions.pop()
        this.stateType = 'question'
        this.currentState = previousQuestion.questionIndex
        this.bankedHours -= json
            .questions[previousQuestion.questionIndex]
            .answers[previousQuestion.answerIndex]
            .addedHours
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
