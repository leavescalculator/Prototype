<template>
  <div class="hello">
    <div v-if="stateType === 'endpoint'">
        <a :href="currentState.url">
            <h3>{{currentState.title}}</h3>
        </a>
    </div>
    <div v-else-if="stateType === 'question'">
        <h3>{{currentState.title}}</h3>
        <p>{{currentState.description}}</p>
        <div v-for="answer in currentState.answers">
            <button v-on:click="changeState(answer.nextState)">{{ answer.title }}</button>
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
            jsonData: json
        }
  },
  methods: {
    changeState: function(nextState) {
        this.stateType = nextState.type
        if(this.stateType === 'question') {
            this.currentState = json.questions[nextState.index]
        } else if(this.stateType === 'endpoint') {
            this.currentState = json.endpoints[nextState.index]
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
