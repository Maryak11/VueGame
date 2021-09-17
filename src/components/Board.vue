<template>
  <div class="board-wrapper">
    <div class="board">
      <BoardItem
        v-for="item in fields"
        :field="item"
        :preview="preview"
        :key="item.id"
        :gameStatus="gameStatus"
        @checkItem="changeItem($event)"
      />
    </div>
    {{ error }}
    <p class="diff">
      Сложность <strong>{{ diff }}</strong>
    </p>
    <button class="btn" @click="start">Старт</button>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import BoardItem from './BoardItem.vue'
import { onBeforeMount } from '@vue/runtime-core'
import { GAME_STATUS, ERRORS } from '../constans'
export default {
  components: { BoardItem },
  setup() {
    const diff = ref(3)
    const fields = ref([])
    let val = ref(0)

    const gameStatus = ref(GAME_STATUS.NONE)
    const number = 25

    const init = () => {
      fields.value = []

      for (let i = 0; i < number; i++) {
        fields.value.push({
          id: i,
          clicked: false,
          value: 0
        })
      }
    }

    onBeforeMount(init)

    return {
      diff,
      fields,
      number,
      error: '',
      gameStatus,
      init,
      val
    }
  },
  methods: {
    start() {
      this.init()
      this.prepareGame()
    },
    prepareGame() {
      this.gameStatus = GAME_STATUS.PREVIEW
      for (let i = 0; i < this.diff; i++) {
        const index = this.rand(0, this.number - 1)
        if (this.fields[index].value !== 1) {
          this.fields[index].value = 1
        } else {
          i--
        }
      }
      console.log(this.fields)
      setTimeout(() => {
        this.gameStatus = GAME_STATUS.STARTED
      }, 2000)
    },
    rand(min, max) {
      return Math.floor(Math.random() * (max - min)) + min
    },
    changeItem(id) {
      this.fields = this.fields.map(el =>
        el.id === id ? { ...el, clicked: !el.clicked } : { ...el }
      )
      this.fields.forEach(el => {
        if (el.id === id) {
          if (el.value === 1) {
            this.val++
            if (this.val === this.diff) {
              this.diff++
              this.val = 0
              setTimeout(() => {
                this.init()
              }, 1000)
              setTimeout(() => {
                this.prepareGame()
              }, 1100)
            }
          } else {
            this.error = ERRORS.FAIL
            this.val = 0
            this.diff = 3
            setTimeout(() => {
              this.error = ''
              this.init()
            }, 1100)
          }
        }
      })
    }
  }
}
</script>

<style lang="scss">
.board-wrapper {
  margin-bottom: 40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.board {
  display: block;
  width: 300px;
  background: #eee;
  margin: 0 auto;
}
.btn {
  background: darkorchid;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 50px;
  margin: 10px 0;
  cursor: pointer;
}
</style>
