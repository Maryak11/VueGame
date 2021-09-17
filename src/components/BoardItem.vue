<template>
  <div :class="activeClass" class="item" @click="checkItem(field.id)"></div>
</template>

<script>
import { computed } from '@vue/reactivity'
import { GAME_STATUS } from '../constans'
export default {
  props: {
    field: {
      type: Object,
      required: true
    },
    gameStatus: {
      type: Number,
      required: true
    }
    // 89992388119 Коля
  },
  setup(props) {
    const activeClass = computed(() => {
      if (props.field.value === 1 && props.gameStatus === GAME_STATUS.PREVIEW) {
        return 'item2'
      } else if (
        props.field.clicked === true &&
        props.gameStatus === GAME_STATUS.STARTED &&
        props.field.value !== 1
      ) {
        return 'item1'
      } else if (
        props.field.clicked === true &&
        props.gameStatus === GAME_STATUS.STARTED &&
        props.field.value === 1
      ) {
        return 'item2'
      }
    })
    return {
      activeClass
    }
  },
  methods: {
    checkItem(id) {
      this.$emit('checkItem', id)
    }
  }
}
</script>

<style lang="scss">
.item {
  position: relative;
  cursor: pointer;
  width: 50px;
  height: 50px;
  background: #ccc;
  margin: 5px;
  cursor: pointer;
  float: left;
  transition: 0.4s;
  transform-style: preserve-3d;
  &.item2 {
    background: rgb(43, 220, 20);
    transform: rotateX(180deg);
  }
  &.item1 {
    background: rgb(220, 20, 63);
    transform: rotateX(180deg);
  }
}
</style>
