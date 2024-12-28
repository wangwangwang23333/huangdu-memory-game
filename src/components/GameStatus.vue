<template>
  <div>
    <div class="status-footer">
      <span v-if="status === IStatus.READY">准备</span>
      <span v-if="status === IStatus.PLAYING">游戏中</span>
      <a v-if="status === IStatus.PASSED" @click.prevent.stop="reset">再次尝试</a>
      <span class="time-cost">{{ timeCost }} s</span>
    </div>
    <!--分割线-->
    <div style="border-top: 1px solid #000; margin-top: 5px"></div>
    <div style="text-we">
      <span>在翻小卡牌的时候，在卡牌上轻轻向左滑动哦～</span>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import { useStore } from 'vuex'
import { GameStoreKey } from '@/stores'
import { IStatus } from '@/constants'

const { state, commit } = useStore(GameStoreKey)

const status = computed(() => state.status)
const timeCost = computed(() => state.timeCost)
const reset = () => commit('reset')
</script>

<style scoped>
.status-footer {
  position: relative;
  margin-top: 10px;
  width: 100%;
  height: 20px;
  line-height: 20px;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
}
a {
  text-decoration: none;
  cursor: pointer;
}
.time-cost {
  position: absolute;
  right: 10px;
  font-size: 15px;
  font-weight: normal;
}
</style>
