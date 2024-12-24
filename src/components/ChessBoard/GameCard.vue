<template>
  <div class="container" @mousedown="handleMouseDown" @mousemove="handleMouseMove" @mouseup="handleMouseUp">
    <div class="card" :class="{ flipped: card.flipped }">
      <img v-if="card.name === '8-ball'" class="front" src="../../assets/first.png" draggable="false" />
      <img
        v-if="card.name === 'baked-potato'"
        class="front"
        src="../../assets/second.png"
        draggable="false"
      />
      <img v-if="card.name === 'dinosaur'" class="front" src="../../assets/third.png" draggable="false" />
      <img v-if="card.name === 'kronos'" class="front" src="../../assets/fourth.png" draggable="false" />
      <img v-if="card.name === 'rocket'" class="front" src="../../assets/fifth.png" draggable="false" />
      <img
        v-if="card.name === 'skinny-unicorn'"
        class="front"
        src="../../assets/sixth.png"
        draggable="false"
      />
      <img v-if="card.name === 'that-guy'" class="front" src="../../assets/seventh.png" draggable="false" />
      <img v-if="card.name === 'zeppelin'" class="front" src="../../assets/eighth.png" draggable="false" />

      <img class="back" src="../../assets/back.png" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { toRefs, ref } from 'vue'
import { useStore } from 'vuex'
import type { ICard } from '@/IType'
import { GameStoreKey } from '@/stores'

interface IGameCardProps {
  card: ICard
}

const props = defineProps<IGameCardProps>()

const emit = defineEmits(['onFlip'])

const { card } = toRefs(props)
const { commit } = useStore(GameStoreKey)

// 触摸起始位置
const touchStartX = ref(0)
const touchEndX = ref(0)

// 检测鼠标按下
const handleMouseDown = (event: MouseEvent) => {
  touchStartX.value = event.clientX
}

// 检测鼠标移动
const handleMouseMove = (event: MouseEvent) => {
  touchEndX.value = event.clientX
}

const doFlip = () => {
  if (card.value.flipped) {
    return
  }
  commit('flips', [card.value])
  emit('onFlip', card.value)
}

// 检测鼠标释放
const handleMouseUp = () => {
  if (touchStartX.value - touchEndX.value > 5) {
    // 左滑翻转
    doFlip()
  }
}
</script>

<style scoped>
.container {
  width: 100px;
  height: 121px;
  margin-right: 3px;
  cursor: pointer;
  position: relative;
  perspective: 800px;
}

.card {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
}

.card.flipped {
  transform: rotateY(180deg);
}

.card img {
  display: block;
  height: 100%;
  width: 100%;
  position: absolute;
  backface-visibility: hidden;
}

.card .back {
  background: blue;
  transform: rotateY(0deg);
}

.card .front {
  background: blue;
  transform: rotateY(180deg);
}

@media screen and (max-width: 450px) {
  .container {
    width: 92px;
    height: 111px;
    margin-right: 1px;
  }
}

@media screen and (max-width: 380px) {
  .container {
    width: 85px;
    height: 102px;
    margin-right: 1px;
  }
}

@media screen and (max-width: 360px) {
  .container {
    width: 70px;
    height: 84px;
    margin-right: 1px;
  }
}

img {
  user-drag: none; /* 针对 Safari */
  user-select: none; /* 防止图片被选中 */
  -webkit-user-drag: none; /* 针对 Chrome 和 Safari */
  -moz-user-drag: none; /* 针对 Firefox */
  -ms-user-drag: none; /* 针对 IE */
  -webkit-touch-callout: none; /* 禁用长按弹出菜单 */
}
</style>
