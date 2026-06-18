<template>
  <van-config-provider :theme-vars="themeVars">
    <main v-if="currentPage === 'connect'" class="page-shell connect-page fade-in">
      <header class="brand-header">
        <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
      </header>

      <section class="machine-card" aria-label="注塑机图片">
        <img class="machine-image" src="/machine.jpg" :alt="pageName" />
      </section>

      <section class="machine-info-card" aria-label="设备信息">
        <div class="info-card-inner">
          <p class="info-row">
            <span class="info-label">机身编码:</span>
            <span class="info-value">{{ serialNo }}</span>
          </p>
          <p class="info-row">
            <span class="info-label">机器型号:</span>
            <span class="info-value">{{ modelName }}</span>
          </p>
          <p class="info-row">
            <span class="info-label">机器吨位:</span>
            <span class="info-value">300T</span>
          </p>
          <p class="info-row">
            <span class="info-label">射胶量:</span>
            <span class="info-value">562g</span>
          </p>
          <p class="info-row">
            <span class="info-label">射胶压力:</span>
            <span class="info-value">221 Mpa</span>
          </p>
          <p class="info-row">
            <span class="info-label">保压压力:</span>
            <span class="info-value">177 Mpa</span>
          </p>
        </div>
      </section>

      <section class="countdown-area" :class="{ timeout: isTimeout }" aria-label="倒计时">
        <div class="countdown-ring-wrapper">
          <svg class="countdown-svg" viewBox="0 0 160 160">
            <circle class="ring-bg" cx="80" cy="80" r="70" />
            <circle
              class="ring-progress"
              :class="{ timeout: isTimeout }"
              cx="80" cy="80" r="70"
              :stroke-dasharray="circumference"
              :stroke-dashoffset="dashOffset"
            />
          </svg>
          <div class="countdown-number" :class="{ timeout: isTimeout }">{{ secondsLeft }}</div>
        </div>
        <div class="status-indicator" :class="{ timeout: isTimeout }">
          <span v-if="!isTimeout" class="pulse-dot"></span>
          <p class="connection-message" :class="{ timeout: isTimeout }" v-html="connectionMessage"></p>
        </div>
        <van-button v-show="isTimeout" class="retry-button" type="primary" round @click="retry">
          重试
        </van-button>
      </section>

      <footer class="action-bar">
        <van-button class="action-button cancel-button" block @click="cancel">
          取消连接
        </van-button>
      </footer>
    </main>

    <Setup2
      v-else-if="currentPage === 'setup2'"
      @back="backToConnect"
      @next="goToSetup3"
    />

    <Setup3
      v-else-if="currentPage === 'setup3'"
      :product="productData"
      @back="backToSetup2"
      @next="goToSetup4"
    />

    <Setup4
      v-else-if="currentPage === 'setup4'"
      :serial-no="serialNo"
      :model-name="modelName"
      :product="productData"
      :params="paramsData"
      @back="backToSetup3"
      @next="goToSetup5"
    />

    <Setup5
      v-else
      @back="backToSetup4"
    />
  </van-config-provider>
</template>

<script setup>
import { computed, onBeforeUnmount, ref } from 'vue'
import { showToast } from 'vant'
import Setup2 from './setup2.vue'
import Setup3 from './setup3.vue'
import Setup4 from './setup4.vue'
import Setup5 from './setup5.vue'

const pageName = '注塑机图片'
const serialNo = 'DH-2025001'
const modelName = '360HD'
const currentPage = ref('connect')
const secondsLeft = ref(30)
const isTimeout = computed(() => secondsLeft.value === 0)
const connectionMessage = computed(() =>
  isTimeout.value ? '连接超时' : '正在连接注塑机...<br>请在设备上允许操作'
)

const TOTAL_SECONDS = 30
const circumference = 2 * Math.PI * 70 // r=70
const dashOffset = computed(() => {
  const progress = secondsLeft.value / TOTAL_SECONDS
  return circumference * (1 - progress)
})

const themeVars = {
  primaryColor: '#69bd00',
  buttonPrimaryBackground: '#69bd00',
  buttonPrimaryBorderColor: '#69bd00',
  buttonBorderRadius: '0px'
}

// 共享数据
const productData = ref({
  material: 'PP',
  totalWeight: 50,
  wallThickness: 50,
  cavityCount: 20
})

const paramsData = ref({
  speedOne: 20,
  speedTwo: 20,
  activeCategory: '注射速度'
})

const timer = window.setInterval(() => {
  if (currentPage.value === 'connect' && secondsLeft.value > 0) {
    secondsLeft.value -= 1
  }
}, 1000)

onBeforeUnmount(() => {
  window.clearInterval(timer)
})

function cancel() {
  currentPage.value = 'setup2'
  showToast('已取消连接')
}

function retry() {
  secondsLeft.value = 30
  showToast('正在重试连接')
}

function backToConnect() {
  currentPage.value = 'connect'
  secondsLeft.value = 30
}

function goToSetup3(data) {
  if (data) {
    productData.value = { ...data }
  }
  currentPage.value = 'setup3'
}

function backToSetup2() {
  currentPage.value = 'setup2'
}

function goToSetup4(data) {
  if (data) {
    paramsData.value = { ...data }
  }
  currentPage.value = 'setup4'
}

function backToSetup3() {
  currentPage.value = 'setup3'
}

function goToSetup5() {
  currentPage.value = 'setup5'
}

function backToSetup4() {
  currentPage.value = 'setup4'
}
</script>
