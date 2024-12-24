<template>
  <div
    class="game-panel"
    :class="{ flipped: isFlipped }"
    @mousedown="handleMouseDown"
    @mousemove="handleMouseMove"
    @mouseup="handleMouseUp"
  >
    <!-- 正面内容 -->
    <div>
      <ScoreBoard style="backface-visibility: hidden" />
      <ChessBoard style="backface-visibility: hidden" />
      <GameStatus style="backface-visibility: hidden" />
    </div>

    <!-- 背面内容 -->
    <div class="game-panel-back">
      <h2>黄渡游戏公司开发</h2>
      <div>
        访问官网：<a href="https://wangwangwang.website/huangdu-game/" target="_blank"
          >https://wangwangwang.website/huangdu-game/</a
        >
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, computed } from 'vue'
import { useStore } from 'vuex'
import { ScoreBoard, ChessBoard, GameStatus } from '@/components'
import { GameStoreKey } from '@/stores'

// 是否翻转的状态
const isFlipped = ref(false)

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

// 检测鼠标释放
const handleMouseUp = () => {
  if (touchStartX.value - touchEndX.value > 150) {
    // 左滑翻转
    doFilp()
  }
}

const { commit } = useStore(GameStoreKey)
onMounted(() => {
  commit('reset')
})
const { state } = useStore(GameStoreKey)
const realtimeNonMatchedPairs = computed(() => state.nonMatchedPairs)
const doFilp = () => {
  // 如果已经翻转过了，恢复原状态
  if (isFlipped.value) {
    isFlipped.value = false
    console.log('Game panel flipped back')
    return
  }

  // 判断是否所有卡牌都已经翻开
  // 直接从IState中获取nonMatchedPairs
  if (realtimeNonMatchedPairs.value === 0) {
    console.log('All cards are flipped')
    isFlipped.value = true
    console.log('Game panel flipped')
  } else {
    console.log(`There are ${realtimeNonMatchedPairs.value} pairs of cards not matched`)
  }
  isFlipped.value = true
}
</script>

<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

html,
body {
  width: 100%;
  height: 100%;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-panel {
  width: 450px;
  height: 670px;
  border: 4px solid #bdbdbd;
  border-radius: 2px;
  background-color: #faf8ef;
  padding: 10px;
  display: flex;
  flex-direction: column;
  transition: transform 0.6s; /* 添加平滑翻转动画 */
  transform-style: preserve-3d; /* 保持 3D 效果 */
}

.game-panel.flipped {
  transform: rotateY(180deg); /* 添加翻转效果 */
}

.game-panel-front,
.game-panel-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden; /* 隐藏背面 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-panel-front {
  background-color: #faf8ef; /* 正面背景颜色 */
  z-index: 2; /* 正面优先显示 */
}

.game-panel-back {
  background-color: white; /* 背面背景颜色 */
  transform: rotateY(180deg); /* 翻转背面 */
  z-index: 1; /* 背面默认隐藏 */
}

@media screen and (max-width: 450px) {
  .game-panel {
    width: 100%;
    height: 100%;
    justify-content: space-around;
  }
}
</style>
