<template>
  <main class="page-shell setup3-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="setup3-hero">
      <h1 class="setup3-title">工艺参数设置</h1>
      <p class="setup3-subtitle">选择参数类别，调整具体数值</p>
    </section>

    <nav class="category-tabs" aria-label="参数分类">
      <button
        v-for="cat in categories"
        :key="cat"
        class="category-tab"
        :class="{ active: activeCategory === cat }"
        type="button"
        @click="activeCategory = cat"
      >
        <span class="tab-icon" aria-hidden="true">
          <svg v-if="cat === '料筒温度'" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 14.76V3.5a2.5 2.5 0 0 0-5 0v11.26a4.5 4.5 0 1 0 5 0z"/></svg>
          <svg v-else-if="cat === '注射速度'" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
          <svg v-else-if="cat === '注射压力'" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg>
          <svg v-else-if="cat === '保压'" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
          <svg v-else width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 2v6h.01M6 10v2M6 14v2M6 18v2M6 22v-2M12 2v4M12 8v2M12 12v2M12 16v2M12 20v2M18 2v3M18 8v2M18 14v2M18 18v2"/><circle cx="6" cy="8" r="2"/><circle cx="12" cy="6" r="2"/><circle cx="18" cy="5" r="2"/></svg>
        </span>
        <span class="tab-text">{{ cat }}</span>
      </button>
    </nav>

    <section class="param-section">
      <div class="param-section-header">
        <span class="param-section-title">{{ activeCategory }}</span>
      </div>

      <!-- 料筒温度 -->
      <div v-if="activeCategory === '料筒温度'" class="param-group">
        <div class="param-row">
          <span class="param-name">一段 (加料段)</span>
          <div class="param-control">
            <van-stepper v-model="barrelTemp.zone1" class="param-stepper" integer :min="100" :max="350" />
            <span class="param-unit">°C</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">二段 (压缩段)</span>
          <div class="param-control">
            <van-stepper v-model="barrelTemp.zone2" class="param-stepper" integer :min="100" :max="350" />
            <span class="param-unit">°C</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">三段 (计量段)</span>
          <div class="param-control">
            <van-stepper v-model="barrelTemp.zone3" class="param-stepper" integer :min="100" :max="350" />
            <span class="param-unit">°C</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">喷嘴温度</span>
          <div class="param-control">
            <van-stepper v-model="barrelTemp.nozzle" class="param-stepper" integer :min="100" :max="350" />
            <span class="param-unit">°C</span>
          </div>
        </div>
      </div>

      <!-- 注射速度 -->
      <div v-else-if="activeCategory === '注射速度'" class="param-group">
        <div class="param-row">
          <span class="param-name">一段速度</span>
          <div class="param-control">
            <van-stepper v-model="injectSpeed.stage1" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">二段速度</span>
          <div class="param-control">
            <van-stepper v-model="injectSpeed.stage2" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">三段速度</span>
          <div class="param-control">
            <van-stepper v-model="injectSpeed.stage3" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">射胶终点位置</span>
          <div class="param-control">
            <van-stepper v-model="injectSpeed.vpPosition" class="param-stepper" integer :min="0" :max="200" />
            <span class="param-unit">mm</span>
          </div>
        </div>
      </div>

      <!-- 注射压力 -->
      <div v-else-if="activeCategory === '注射压力'" class="param-group">
        <div class="param-row">
          <span class="param-name">一段压力</span>
          <div class="param-control">
            <van-stepper v-model="injectPressure.stage1" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">二段压力</span>
          <div class="param-control">
            <van-stepper v-model="injectPressure.stage2" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">三段压力</span>
          <div class="param-control">
            <van-stepper v-model="injectPressure.stage3" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">最大射胶压力</span>
          <div class="param-control">
            <van-stepper v-model="injectPressure.maxPressure" class="param-stepper" integer :min="50" :max="300" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>

      <!-- 保压 -->
      <div v-else-if="activeCategory === '保压'" class="param-group">
        <div class="param-row">
          <span class="param-name">保压压力</span>
          <div class="param-control">
            <van-stepper v-model="holdingPressure.pressure" class="param-stepper" integer :min="10" :max="200" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">保压时间</span>
          <div class="param-control">
            <van-stepper v-model="holdingPressure.time" class="param-stepper" integer :min="0" :max="60" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">背压</span>
          <div class="param-control">
            <van-stepper v-model="holdingPressure.backPressure" class="param-stepper" integer :min="0" :max="50" />
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>

      <!-- 冷却时间 -->
      <div v-else class="param-group">
        <div class="param-row">
          <span class="param-name">冷却时间</span>
          <div class="param-control">
            <van-stepper v-model="cooling.time" class="param-stepper" integer :min="1" :max="120" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">顶出延时</span>
          <div class="param-control">
            <van-stepper v-model="cooling.ejectDelay" class="param-stepper" integer :min="0" :max="30" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
        </div>
        <div class="param-row">
          <span class="param-name">模温设定</span>
          <div class="param-control">
            <van-stepper v-model="cooling.moldTemp" class="param-stepper" integer :min="20" :max="180" />
            <span class="param-unit">°C</span>
          </div>
        </div>
      </div>
    </section>

    <footer class="action-bar split-action-bar">
      <van-button class="action-button secondary-button" block @click="emit('back')">
        上一步
      </van-button>
      <van-button class="action-button confirm-button" block @click="confirmParams">
        开始生产
      </van-button>
    </footer>

    <div v-if="showLoading" class="ai-loading-mask" aria-live="polite">
      <div class="ai-loading-panel">
        <div class="ai-spinner" aria-hidden="true"></div>
        <p class="ai-loading-text">设备正在生产中</p>
      </div>
    </div>
  </main>
</template>

<script setup>
import { reactive, ref, onBeforeUnmount } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back', 'next'])

const categories = ['料筒温度', '注射速度', '注射压力', '保压', '冷却时间']
const activeCategory = ref('注射速度')

const showLoading = ref(false)
let loadingTimer = null

onBeforeUnmount(() => {
  if (loadingTimer) window.clearTimeout(loadingTimer)
})

const barrelTemp = reactive({
  zone1: 180,
  zone2: 200,
  zone3: 220,
  nozzle: 215
})

const injectSpeed = reactive({
  stage1: 30,
  stage2: 50,
  stage3: 35,
  vpPosition: 45
})

const injectPressure = reactive({
  stage1: 80,
  stage2: 100,
  stage3: 70,
  maxPressure: 160
})

const holdingPressure = reactive({
  pressure: 60,
  time: 8,
  backPressure: 5
})

const cooling = reactive({
  time: 25,
  ejectDelay: 2,
  moldTemp: 60
})

function confirmParams() {
  showLoading.value = true
  loadingTimer = window.setTimeout(() => {
    showLoading.value = false
    emit('next', {
      barrelTemp: { ...barrelTemp },
      injectSpeed: { ...injectSpeed },
      injectPressure: { ...injectPressure },
      holdingPressure: { ...holdingPressure },
      cooling: { ...cooling },
      activeCategory: activeCategory.value
    })
  }, 3000)
}
</script>

<style scoped>
.setup3-page {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding-bottom: 120px;
}

.setup3-hero {
  padding: 8px 4px 4px;
}

.setup3-title {
  margin: 0;
  color: #1f2420;
  font-size: clamp(22px, 6vw, 30px);
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: 0;
  text-align: center;
}

.setup3-subtitle {
  margin: 4px 0 0;
  color: rgba(31, 36, 32, 0.5);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 400;
  line-height: 1.4;
  text-align: center;
}

.category-tabs {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 0;
  border: 1px solid rgba(105, 189, 0, 0.14);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.78);
  box-shadow: 0 4px 16px rgba(105, 189, 0, 0.06);
  overflow: hidden;
}

.category-tab {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  min-width: 0;
  min-height: 64px;
  padding: 8px 6px;
  border: 0;
  border-right: 1px solid rgba(31, 36, 32, 0.06);
  color: rgba(31, 36, 32, 0.55);
  background: rgba(105, 189, 0, 0.02);
  font-size: clamp(11px, 3vw, 13px);
  font-weight: 500;
  line-height: 1.15;
  text-align: center;
  cursor: pointer;
  transition: all 0.15s ease;
}

.category-tab:last-child {
  border-right: 0;
}

.category-tab:hover {
  background: rgba(105, 189, 0, 0.04);
}

.category-tab.active {
  color: #69bd00;
  font-weight: 700;
  background: rgba(105, 189, 0, 0.1);
  box-shadow: inset 0 -3px 0 #69bd00;
}

.tab-icon {
  display: grid;
  place-items: center;
  flex: 0 0 auto;
}

.tab-text {
  font-size: clamp(11px, 3vw, 13px);
  font-weight: 500;
  line-height: 1.15;
  text-align: center;
}

.category-tab.active .tab-text {
  font-weight: 600;
}

.param-section {
  padding: 0 2px;
}

.param-section-header {
  padding: 6px 4px 8px;
  border-bottom: 1px solid rgba(105, 189, 0, 0.15);
  margin-bottom: 8px;
}

.param-section-title {
  color: #1f2420;
  font-size: clamp(15px, 4.4vw, 19px);
  font-weight: 600;
}

.param-group {
  display: grid;
  gap: 8px;
}

.param-row {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 12px;
  border: 1px solid rgba(105, 189, 0, 0.1);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.7);
}

.param-name {
  flex: 0 0 7em;
  color: rgba(31, 36, 32, 0.7);
  font-size: clamp(13px, 3.8vw, 16px);
  font-weight: 500;
  line-height: 1.2;
}

.param-control {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  flex: 0 0 auto;
}

.param-unit {
  color: rgba(31, 36, 32, 0.45);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1;
  white-space: nowrap;
}

.param-stepper :deep(.van-stepper__minus),
.param-stepper :deep(.van-stepper__plus) {
  width: 30px;
  height: 30px;
  border-radius: 6px;
  color: #fff;
  background: #9bd969;
}

.param-stepper :deep(.van-stepper__input) {
  width: 48px;
  height: 30px;
  margin: 0 4px;
  color: #1f2420;
  background: rgba(105, 189, 0, 0.06);
  border: 1px solid rgba(105, 189, 0, 0.12);
  border-radius: 6px;
  font-size: clamp(13px, 3.8vw, 16px);
  font-weight: 600;
}

.ai-loading-mask {
  position: fixed;
  inset: 0;
  z-index: 20;
  display: grid;
  place-items: center;
  padding: 24px;
  background: rgba(255, 255, 255, 0.76);
  backdrop-filter: blur(8px);
}

.ai-loading-panel {
  display: grid;
  justify-items: center;
  gap: 18px;
  width: min(82vw, 300px);
  padding: 34px 24px;
  border: 1px solid rgba(105, 189, 0, 0.22);
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 18px 42px rgba(31, 36, 32, 0.18);
}

.ai-spinner {
  width: 64px;
  height: 64px;
  border: 5px solid rgba(105, 189, 0, 0.16);
  border-top-color: #69bd00;
  border-radius: 50%;
  animation: ai-spin 0.9s linear infinite;
}

.ai-loading-text {
  margin: 0;
  color: #1f2420;
  font-size: clamp(17px, 4.8vw, 21px);
  font-weight: 600;
  line-height: 1.35;
  text-align: center;
}

@keyframes ai-spin {
  to { transform: rotate(360deg); }
}
</style>
