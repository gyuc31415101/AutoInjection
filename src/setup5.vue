<template>
  <main class="page-shell setup5-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="setup5-hero">
      <h1 class="page-title">调整工艺参数</h1>
      <p class="page-subtitle">对比 AI 建议值与当前参数，逐项确认后再进入下一步。</p>
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
<div class="compare-header">
      <span class="compare-label ai-label">AI 优化参数</span>
      <span class="compare-label current-label">当前设备参数</span>
    </div>

    <!-- 料筒温度 -->
    <div v-if="activeCategory === '料筒温度'" class="param-group">
      <div class="param-row" v-for="z in ['一段', '二段', '三段', '喷嘴']" :key="z">
        <span class="param-name">{{ z }}</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.barrel[z].temp" class="param-stepper" integer :min="100" :max="350" />
            <span class="param-unit">°C</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.barrel[z].temp }}</span>
            <span class="param-unit">°C</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 注射速度 -->
    <div v-else-if="activeCategory === '注射速度'" class="param-group">
      <div class="param-row">
        <span class="param-name">一段速度</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectSpeed.stage1" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectSpeed.stage1 }}</span>
            <span class="param-unit">%</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">二段速度</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectSpeed.stage2" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectSpeed.stage2 }}</span>
            <span class="param-unit">%</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">三段速度</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectSpeed.stage3" class="param-stepper" integer :min="1" :max="99" />
            <span class="param-unit">%</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectSpeed.stage3 }}</span>
            <span class="param-unit">%</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">射胶终点位置</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectSpeed.vpPosition" class="param-stepper" integer :min="0" :max="200" />
            <span class="param-unit">mm</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectSpeed.vpPosition }}</span>
            <span class="param-unit">mm</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 注射压力 -->
    <div v-else-if="activeCategory === '注射压力'" class="param-group">
      <div class="param-row">
        <span class="param-name">一段压力</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectPressure.stage1" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectPressure.stage1 }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">二段压力</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectPressure.stage2" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectPressure.stage2 }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">三段压力</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectPressure.stage3" class="param-stepper" integer :min="10" :max="250" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectPressure.stage3 }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">最大射胶压力</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.injectPressure.maxPressure" class="param-stepper" integer :min="50" :max="300" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.injectPressure.maxPressure }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 保压 -->
    <div v-else-if="activeCategory === '保压'" class="param-group">
      <div class="param-row">
        <span class="param-name">保压压力</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.holdingPressure.pressure" class="param-stepper" integer :min="10" :max="200" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.holdingPressure.pressure }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">保压时间</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.holdingPressure.time" class="param-stepper" integer :min="0" :max="60" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.holdingPressure.time }}</span>
            <span class="param-unit">s</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">背压</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.holdingPressure.backPressure" class="param-stepper" integer :min="0" :max="50" />
            <span class="param-unit">MPa</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.holdingPressure.backPressure }}</span>
            <span class="param-unit">MPa</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 冷却时间 -->
    <div v-else class="param-group">
      <div class="param-row">
        <span class="param-name">冷却时间</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.cooling.time" class="param-stepper" integer :min="1" :max="120" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.cooling.time }}</span>
            <span class="param-unit">s</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">顶出延时</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.cooling.ejectDelay" class="param-stepper" integer :min="0" :max="30" :step="0.5" />
            <span class="param-unit">s</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.cooling.ejectDelay }}</span>
            <span class="param-unit">s</span>
          </div>
        </div>
      </div>
      <div class="param-row">
        <span class="param-name">模温设定</span>
        <div class="param-values">
          <div class="param-value ai-value">
            <van-stepper v-model="ai.cooling.moldTemp" class="param-stepper" integer :min="20" :max="180" />
            <span class="param-unit">°C</span>
          </div>
          <div class="param-value current-value">
            <span class="current-num">{{ current.cooling.moldTemp }}</span>
            <span class="param-unit">°C</span>
          </div>
        </div>
      </div>
    </div>

    <div v-if="changedParams.length > 0" class="ai-summary-foot">AI已调整参数：{{ changedParams.slice(0, 3).join('、') }}<span v-if="changedParams.length > 3"> 等{{ changedParams.length }}项</span></div>

    <footer class="action-bar split-action-bar">
      <van-button class="action-button secondary-button" block @click="emit('back')">
        上一步
      </van-button>
      <van-button class="action-button confirm-button" block @click="submitParams">
        确认
      </van-button>
    </footer>
  </main>
</template>

<script setup>
import { computed, reactive, ref } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back'])

const categories = ['料筒温度', '注射速度', '注射压力', '保压', '冷却时间']
const activeCategory = ref('料筒温度')

const ai = reactive({
  barrel: {
    一段: { temp: 180 },
    二段: { temp: 200 },
    三段: { temp: 220 },
    喷嘴: { temp: 215 }
  },
  injectSpeed: {
    stage1: 35,
    stage2: 55,
    stage3: 40,
    vpPosition: 42
  },
  injectPressure: {
    stage1: 85,
    stage2: 105,
    stage3: 75,
    maxPressure: 165
  },
  holdingPressure: {
    pressure: 65,
    time: 10,
    backPressure: 6
  },
  cooling: {
    time: 22,
    ejectDelay: 1.5,
    moldTemp: 55
  }
})

const current = reactive({
  barrel: {
    一段: { temp: 175 },
    二段: { temp: 195 },
    三段: { temp: 210 },
    喷嘴: { temp: 205 }
  },
  injectSpeed: {
    stage1: 30,
    stage2: 50,
    stage3: 35,
    vpPosition: 45
  },
  injectPressure: {
    stage1: 80,
    stage2: 100,
    stage3: 70,
    maxPressure: 160
  },
  holdingPressure: {
    pressure: 60,
    time: 8,
    backPressure: 5
  },
  cooling: {
    time: 25,
    ejectDelay: 2,
    moldTemp: 60
  }
})


const changedParams = computed(() => {
  const changes = []
  const cm = current
  const am = ai
  
  // 料筒温度
  for (const z of ['一段', '二段', '三段', '喷嘴']) {
    if (am.barrel[z].temp !== cm.barrel[z].temp) {
      changes.push(`料筒${z}温度 ${cm.barrel[z].temp}→${am.barrel[z].temp}°C`)
    }
  }
  // 注射速度
  if (am.injectSpeed.stage1 !== cm.injectSpeed.stage1) changes.push(`一段速度 ${cm.injectSpeed.stage1}→${am.injectSpeed.stage1}%`)
  if (am.injectSpeed.stage2 !== cm.injectSpeed.stage2) changes.push(`二段速度 ${cm.injectSpeed.stage2}→${am.injectSpeed.stage2}%`)
  if (am.injectSpeed.stage3 !== cm.injectSpeed.stage3) changes.push(`三段速度 ${cm.injectSpeed.stage3}→${am.injectSpeed.stage3}%`)
  if (am.injectSpeed.vpPosition !== cm.injectSpeed.vpPosition) changes.push(`射胶终点 ${cm.injectSpeed.vpPosition}→${am.injectSpeed.vpPosition}mm`)
  // 注射压力
  if (am.injectPressure.stage1 !== cm.injectPressure.stage1) changes.push(`一段压力 ${cm.injectPressure.stage1}→${am.injectPressure.stage1}MPa`)
  if (am.injectPressure.stage2 !== cm.injectPressure.stage2) changes.push(`二段压力 ${cm.injectPressure.stage2}→${am.injectPressure.stage2}MPa`)
  if (am.injectPressure.stage3 !== cm.injectPressure.stage3) changes.push(`三段压力 ${cm.injectPressure.stage3}→${am.injectPressure.stage3}MPa`)
  if (am.injectPressure.maxPressure !== cm.injectPressure.maxPressure) changes.push(`最大压力 ${cm.injectPressure.maxPressure}→${am.injectPressure.maxPressure}MPa`)
  // 保压
  if (am.holdingPressure.pressure !== cm.holdingPressure.pressure) changes.push(`保压压力 ${cm.holdingPressure.pressure}→${am.holdingPressure.pressure}MPa`)
  if (am.holdingPressure.time !== cm.holdingPressure.time) changes.push(`保压时间 ${cm.holdingPressure.time}→${am.holdingPressure.time}s`)
  if (am.holdingPressure.backPressure !== cm.holdingPressure.backPressure) changes.push(`背压 ${cm.holdingPressure.backPressure}→${am.holdingPressure.backPressure}MPa`)
  // 冷却
  if (am.cooling.time !== cm.cooling.time) changes.push(`冷却时间 ${cm.cooling.time}→${am.cooling.time}s`)
  if (am.cooling.ejectDelay !== cm.cooling.ejectDelay) changes.push(`顶出延时 ${cm.cooling.ejectDelay}→${am.cooling.ejectDelay}s`)
  if (am.cooling.moldTemp !== cm.cooling.moldTemp) changes.push(`模温 ${cm.cooling.moldTemp}→${am.cooling.moldTemp}°C`)
  
  return changes
})

function submitParams() {
  showToast('工艺参数已确认')
}
</script>

<style scoped>
.setup5-page {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding-bottom: 120px;
}

.setup5-hero {
  padding: 8px 4px 4px;
}

.page-title {
  margin: 0;
  color: #1f2420;
  font-size: clamp(22px, 6vw, 30px);
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: 0;
  text-align: center;
}

.page-subtitle {
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
  padding: 0;
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
  border-bottom: 1px solid rgba(31, 36, 32, 0.06);
  border-right: 1px solid rgba(31, 36, 32, 0.06);
  border-radius: 0;
  color: rgba(31, 36, 32, 0.55);
  background: rgba(105, 189, 0, 0.02);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1.15;
  text-align: center;
  cursor: pointer;
  transition: all 0.15s ease;
}
.category-tab:hover {
  background: rgba(105, 189, 0, 0.04);
}
.category-tab:last-child {
  border-right: 0;
}

.category-tab.active {
  color: #69bd00;
  font-weight: 700;
  border-color: transparent;
  background: rgba(105, 189, 0, 0.1);
  box-shadow: inset 0 -3px 0 #69bd00;
}

.compare-header {
  display: flex;
  justify-content: flex-end;
  gap: 0;
  padding: 0 4px;
}

.compare-label {
  flex: 0 0 calc(50% - 32px);
  text-align: center;
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 600;
  line-height: 1;
  padding: 6px 0;
}

.ai-label {
  color: #69bd00;
  border-bottom: 2px solid #69bd00;
}

.current-label {
  color: rgba(31, 36, 32, 0.5);
  border-bottom: 2px solid rgba(31, 36, 32, 0.18);
}

.param-group {
  display: grid;
  gap: 6px;
  padding: 0 2px;
}

.param-row {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 12px;
  border: 1px solid rgba(105, 189, 0, 0.1);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.7);
}

.param-name {
  flex: 0 0 5em;
  color: rgba(31, 36, 32, 0.7);
  font-size: clamp(13px, 3.8vw, 16px);
  font-weight: 500;
  line-height: 1.2;
}

.param-values {
  display: flex;
  flex: 1;
  gap: 0;
  min-width: 0;
  position: relative;
}
.param-values::after {
  content: "";
  position: absolute;
  left: 50%;
  top: 2px;
  bottom: 2px;
  width: 1px;
  background: rgba(105, 189, 0, 0.2);
}

.param-value {
  display: inline-flex;
  align-items: center;
  justify-content: flex-start;
  gap: 4px;
  flex: 1;
  min-width: 0;
}

.param-value.current-value {
  justify-content: center;
}


.param-unit {
  color: rgba(31, 36, 32, 0.4);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1;
  white-space: nowrap;
}

.param-stepper :deep(.van-stepper__minus),
.param-stepper :deep(.van-stepper__plus) {
  width: 26px;
  height: 26px;
  min-width: 26px;
  padding: 0;
  box-sizing: border-box;
  line-height: 1;
  font-size: 13px;
  font-weight: 700;
  text-align: center;
  border-radius: 6px;
  color: #fff;
  background: #9bd969;
  flex: 0 0 auto;
}

.param-stepper :deep(.van-stepper__input) {
  width: 38px;
  height: 26px;
  margin: 0 2px;
  padding: 0 2px;
  text-align: center;
  color: #1f2420;
  background: rgba(105, 189, 0, 0.06);
  border: 1px solid rgba(105, 189, 0, 0.12);
  border-radius: 6px;
  font-size: 13px;
  font-weight: 600;
  flex: 0 0 auto;
}




.current-num {
  color: rgba(31, 36, 32, 0.7);
  font-size: clamp(13px, 3.8vw, 16px);
  font-weight: 600;
  line-height: 1;
  text-align: center; }

.ai-summary-foot {
  text-align: center;
  padding: 8px 12px;
  font-size: clamp(13px, 3.6vw, 15px);
  color: rgba(31, 36, 32, 0.65);
  line-height: 1.4;
}
.ai-summary-foot .ai-summary-list { color: #69bd00; }
</style>
