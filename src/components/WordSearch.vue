<template>
  <div class="word-search">
    <div class="word-input" v-if="!puzzleGenerated">
      <input v-model="newWord" @keyup.enter="addWord" placeholder="Ingresa una palabra">
      <button @click="addWord">Agregar palabra</button>
    </div>
    <div class="word-list">
      <p>Palabras a encontrar:</p>
      <ul>
        <li v-for="word in words" :key="word" :class="{ found: foundWords.includes(word) }">
          {{ word }}
        </li>
      </ul>
    </div>
    <button v-if="!puzzleGenerated" @click="generatePuzzle" :disabled="words.length !== 5">Generar sopa de letras</button>
    <div v-if="puzzleGenerated" class="grid">
      <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="row">
        <div
            v-for="(cell, cellIndex) in row"
            :key="cellIndex"
            class="cell"
            :class="{ selected: isSelected(rowIndex, cellIndex) }"
            @click="toggleSelection(rowIndex, cellIndex)"
        >
          {{ cell }}
        </div>
      </div>
    </div>
    <p v-if="puzzleGenerated">Palabra seleccionada: {{ selectedWord }}</p>
    <button v-if="puzzleGenerated" @click="checkWord" :disabled="selectedWord.length === 0">Verificar palabra</button>
    <p v-if="puzzleGenerated && allWordsFound">¡Felicidades! Has encontrado todas las palabras.</p>
  </div>
</template>

<script>
export default {
  name: 'WordSearch',
  data() {
    return {
      grid: [],
      words: [],
      newWord: '',
      puzzleGenerated: false,
      selectedCells: [],
      foundWords: [],
    }
  },
  computed: {
    selectedWord() {
      return this.selectedCells.map(cell => this.grid[cell.row][cell.col]).join('')
    },
    allWordsFound() {
      return this.words.length > 0 && this.words.length === this.foundWords.length
    }
  },
  methods: {
    addWord() {
      if (this.newWord && this.words.length < 5 && !this.words.includes(this.newWord.toUpperCase())) {
        this.words.push(this.newWord.toUpperCase())
        this.newWord = ''
      }
    },
    initializeGrid() {
      this.grid = Array(15).fill().map(() => Array(15).fill(''))
    },
    generatePuzzle() {
      this.initializeGrid()
      this.words.forEach(word => this.placeWord(word))
      this.fillGridWithRandomLetters()
      this.puzzleGenerated = true
    },
    placeWord(word) {
      const directions = [
        [0, 1],   // derecha
        [1, 0],   // abajo
        [0, -1],  // izquierda
        [-1, 0],  // arriba
        [1, 1],   // diagonal abajo-derecha
        [1, -1],  // diagonal abajo-izquierda
        [-1, 1],  // diagonal arriba-derecha
        [-1, -1]  // diagonal arriba-izquierda
      ]

      let placed = false
      while (!placed) {
        const direction = directions[Math.floor(Math.random() * directions.length)]
        const [startX, startY] = this.findValidStart(word, direction)

        if (startX !== -1) {
          this.insertWord(word, startX, startY, direction)
          placed = true
        }
      }
    },
    findValidStart(word, [dx, dy]) {
      const maxTries = 100
      for (let i = 0; i < maxTries; i++) {
        const startX = Math.floor(Math.random() * 15)
        const startY = Math.floor(Math.random() * 15)
        if (this.canPlaceWord(word, startX, startY, dx, dy)) {
          return [startX, startY]
        }
      }
      return [-1, -1]
    },
    canPlaceWord(word, startX, startY, dx, dy) {
      if (startX + dx * (word.length - 1) < 0 || startX + dx * (word.length - 1) >= 15) return false
      if (startY + dy * (word.length - 1) < 0 || startY + dy * (word.length - 1) >= 15) return false

      for (let i = 0; i < word.length; i++) {
        const x = startX + dx * i
        const y = startY + dy * i
        if (this.grid[y][x] !== '' && this.grid[y][x] !== word[i]) return false
      }
      return true
    },
    insertWord(word, startX, startY, [dx, dy]) {
      for (let i = 0; i < word.length; i++) {
        const x = startX + dx * i
        const y = startY + dy * i
        this.grid[y][x] = word[i]
      }
    },
    fillGridWithRandomLetters() {
      const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      for (let i = 0; i < 15; i++) {
        for (let j = 0; j < 15; j++) {
          if (this.grid[i][j] === '') {
            this.grid[i][j] = letters[Math.floor(Math.random() * letters.length)]
          }
        }
      }
    },
    toggleSelection(row, col) {
      const index = this.selectedCells.findIndex(cell => cell.row === row && cell.col === col)
      if (index !== -1) {
        // Si la celda ya está seleccionada, la deseleccionamos
        this.selectedCells.splice(index, 1)
      } else {
        // Si la celda no está seleccionada, la añadimos solo si es adyacente a la última celda seleccionada
        if (this.selectedCells.length === 0 || this.isAdjacent(this.selectedCells[this.selectedCells.length - 1], {row, col})) {
          this.selectedCells.push({row, col})
        }
      }
    },
    isAdjacent(cell1, cell2) {
      const rowDiff = Math.abs(cell1.row - cell2.row)
      const colDiff = Math.abs(cell1.col - cell2.col)
      return (rowDiff <= 1 && colDiff <= 1) && (rowDiff + colDiff > 0)
    },
    isSelected(row, col) {
      return this.selectedCells.some(cell => cell.row === row && cell.col === col)
    },
    checkWord() {
      const selectedWord = this.selectedWord
      if (this.words.includes(selectedWord) && !this.foundWords.includes(selectedWord)) {
        this.foundWords.push(selectedWord)
        this.$nextTick(() => {
          alert(`¡Felicidades! Has encontrado la palabra "${selectedWord}".`)
        })
      } else if (this.foundWords.includes(selectedWord)) {
        alert(`Ya has encontrado la palabra "${selectedWord}" antes.`)
      } else {
        alert(`"${selectedWord}" no es una de las palabras a buscar.`)
      }
      this.selectedCells = []
    }
  }
}
</script>

<style scoped>
.word-search {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.grid {
  display: inline-block;
  margin-top: 20px;
  margin-bottom: 20px;
}
.row {
  display: flex;
}
.cell {
  width: 30px;
  height: 30px;
  border: 1px solid #ccc;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  user-select: none;
  cursor: pointer;
}
.cell.selected {
  background-color: #ffeb3b;
}
.word-input {
  margin-bottom: 10px;
}
.word-list {
  margin-bottom: 10px;
}
.word-list ul {
  list-style-type: none;
  padding: 0;
}
.word-list li.found {
  text-decoration: line-through;
  color: #4caf50;
}
</style>