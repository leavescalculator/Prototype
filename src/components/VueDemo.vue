<template>
  <div class="hello">
    <div>banked hours: {{bankedHours}}</div>
    <div v-if="stateType === 'endpoint'">
        <a :href="currentState.url">
            <h3>{{currentState.title}}</h3>
        </a>
    </div>
    <div v-else-if="stateType === 'question'">
        <h3>{{currentState.title}}</h3>
        <p>{{currentState.description}}</p>
        <div v-for="answer in currentState.answers">
            <button v-on:click="changeState(answer)">{{ answer.title }}</button>
            <p>{{ answer.description }}</p>
        </div>
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
            currentState: json.questions[0],
            stateType: 'question',
            jsonData: json,
            bankedHours: 0
        }
  },
  methods: {
    changeState: function(answer) {
        this.stateType = answer.nextState.type
        this.bankedHours += answer.addedHours
        if(this.stateType === 'question') {
            this.currentState = json.questions[answer.nextState.index]
        } else if(this.stateType === 'endpoint') {
            this.currentState = json.endpoints[answer.nextState.index]
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
