<template lang="pug">
.container
  h2(v-if="winner") Winner: {{ winner }} 🌟
  h2(v-else) Players Move: {{ player }}
  button.btn.btn-success.mb-3(@click="reset") Reset
  .row(v-for="(_, y) in 3" :key="y")
    button.square(v-for="(_, x) in 3" :key="x" @click="move(x, y)") {{ squares[x][y] }}
</template>

<script>
import { computed, ref } from 'vue'

const calculateWinner = squares => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]
    }
  }
  return null
}

export default {
  setup () {
    const player = ref('X')
    const squares = ref([
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ])
    const winner = computed(() => calculateWinner(squares.value.flat()))

    const move = (x, y) => {
      if (winner.value) return
      squares.value[x][y] = player.value
      player.value = player.value === 'O' ? 'X' : 'O'
    }

    const reset = () => {
      player.value = 'X'
      squares.value = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ]
    }

    return { winner, player, squares, move, reset }
  }
}
</script>

<style scoped>
.square {
  background: rgb(255, 255, 255);
  border: 2px solid #998;
  float: left;
  font-size: 70px;
  font-weight: bold;
  line-height: 34px;
  height: 100px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 100px;
}
</style>
