<script setup>
  import { ref, computed, onMounted } from "vue";
  
  let player = ref('O')
  const gamecells = ref(null)
  const cells = ref(['', '', '', '', '', '', '', '', ''])

  // onMounted(() => {
  //   gamecells.value.push(cells)
  // })

 const calculateWinner = (cells) => {
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
    const [a, b, c] = lines[i]; //destructuring
    if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
      return cells[a];
    }
  }
  return null;
}

const winner = computed(() => calculateWinner(cells.value))

const makemove = (x) => {
  if(winner.value) return

  if(cells.value[x] != '') return

  cells.value[x] = player.value
  player.value = player.value === 'O' ? 'X' : 'O' 
}

const reset = () => {
  cells.value = ['', '', '', '', '', '', '', '', '']
  player.value = 'O'
}

const drawCheck = computed(() => {
  if(winner.value !== null) return false
  for (let cell of cells.value) {
    if (cell === '') return false;
  }
  return true
  //return false in forEach: This only skips the current iteration 
  // and does not exit the entire drawCheck function. 
  // The loop will continue for all other cells, and after it finishes, 
  // the function will still reach the return true statement.
})

  
</script>
 
<template>
  <div class="flex flex-col justify-center items-center h-screen">
    <h3 class="font-bold my-4">This is {{ player }}'s turn.</h3>
    <div class="w-60 h-60 bg-black flex flex-wrap">
      <div ref="gamecells" 
        v-for="(cell, index) in cells" :id="index" 
        @click="makemove(index)"
        class="w-20 h-20 bg-white border border-black
        flex justify-center items-center material-icon-outlined text-4xl cursor-pointer hover:bg-slate-100">
        {{ cell }}
      </div>
    </div>
    <h2 v-if="winner !== null" class="text-6xl font-bold my-4">Player {{ winner }} WINS!!</h2>
    <h2 v-if="drawCheck" class="text-6xl font-bold my-4">It's a DRAW!!</h2>
    <button @click="reset" class="px-4 py-2 border-2 border-black rounded-md my-4">Reset Game</button>
  </div>
</template>
 
<style scoped>

</style>