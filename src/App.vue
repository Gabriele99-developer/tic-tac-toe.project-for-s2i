<template>
  <div>
    <div class="scoreBoard">
      <span>X ha {{ wins.X }} vittorie</span>
      <h2>Segnapunti</h2>
      <span>O ha {{ wins.O }} vittorie</span>
    </div>
    
    <div id="app">
      <div id="details">
        <h1>Tic Tac Toe</h1>
        <h2>Partita numero #{{ matches + 1 }}</h2>
      </div>
      <grid></grid>
      <button class="restart" @click="restart">Ricomincia</button>
    </div>
  </div>
</template>

<script>
import Grid from './components/Grid.vue'

export default {
  components: { Grid },
  name: 'app',
  data () {
    return {
      matches: 0,
      wins: {
        O: 0,
        X: 0
      }
    }
  },

  methods: {
    restart () {
      Event.$emit('clearCell')

      Event.$emit('gridReset')

      this.matches++
    }
  },

  created () {
    Event.$on('win', winner => this.wins[winner]++)
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=PT+Sans&display=swap');
body {
  background-color: #fff;
  color: #fff;
  font-family: 'PT+Sans';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
}

#app {
  margin: 0 auto;
  max-width: 270px;
  color: white;
}

h1 {
  color: black;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 3em;
}

.restart {
  background-color: red;
  color: #fff;
  border: 0px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  font-family: 'PT+Sans';
  font-size: 1.4em;
  font-weight: bold;
  margin: 0px;
  padding: 15px;
  width: 100%;
}

.restart:hover {
  cursor: pointer;
}

.scoreBoard {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 15px;
  background-color: red;
  box-shadow: 10px solid #fff;
  padding: 20px;
  overflow-x: none;
}

.scoreBoard h2 {
  margin: 0px;
}

.scoreBoard span {
  float: right;
  font-size: 1.5em;
  font-weight: bold;
  margin-left: 20px;
}
</style>