<template>
  <div class="poll-container">
    <h2>{{ question }}</h2>
    <div v-if="!hasVoted">
      <button 
        v-for="(option, index) in options" 
        :key="index" 
        @click="vote(index)" 
        class="option-button">
        {{ option.text }}
      </button>
    </div>
    <div v-else class="results">
      <div v-for="(option, index) in options" :key="index" class="result-bar">
        <span>{{ option.text }} - {{ option.votes }} votes ({{ getPercentage(index) }}%)</span>
        <div class="progress" :style="{ width: getPercentage(index) + '%' }"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const question = ref("Which is your favorite frontend framework?");
    const options = ref([
      { text: "Vue.js", votes: 0 },
      { text: "React", votes: 0 },
      { text: "Angular", votes: 0 },
      { text: "Svelte", votes: 0 }
    ]);
    const hasVoted = ref(false);

    const vote = (index) => {
      options.value[index].votes++;
      hasVoted.value = true;
    };

    const getPercentage = (index) => {
      const totalVotes = options.value.reduce((sum, option) => sum + option.votes, 0);
      return totalVotes ? ((options.value[index].votes / totalVotes) * 100).toFixed(1) : 0;
    };

    return { question, options, hasVoted, vote, getPercentage };
  }
};
</script>

<style>
.poll-container {
  text-align: center;
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
}
.option-button {
  display: block;
  width: 100%;
  margin: 5px 0;
  padding: 10px;
  border: none;
  background-color: #42b883;
  color: white;
  cursor: pointer;
  border-radius: 5px;
}
.option-button:hover {
  background-color: #369a6e;
}
.results {
  margin-top: 20px;
}
.result-bar {
  text-align: left;
  margin: 10px 0;
}
.progress {
  height: 10px;
  background-color: #42b883;
  transition: width 0.5s ease-in-out;
}
</style>
