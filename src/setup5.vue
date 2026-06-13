<template>
  <main class="page-shell setup5-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="hero-copy">
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
        {{ cat }}
      </button>
    </nav>

    <section v-if="activeCategory === '料筒温度'" class="subtabs-card" aria-label="料筒温度切换">
      <button
        v-for="zone in barrelZones"
        :key="zone"
        class="subtab"
        :class="{ active: activeBarrelZone === zone }"
        type="button"
        @click="activeBarrelZone = zone"
      >
        {{ zone }}
      </button>
    </section>

    <section class="comparison-stack" aria-label="工艺参数对比">
      <article class="panel-card ai-panel">
        <header class="panel-heading">AI优化后工艺参数</header>
        <div class="panel-content">
          <div v-if="activeCategory === '注射速度'" class="param-list">
            <div class="param-row">
              <span class="param-label">参数1</span>
              <div class="param-control">
                <van-stepper v-model="ai.speedOne" class="param-stepper" integer />
              </div>
            </div>
            <div class="param-row">
              <span class="param-label">参数2</span>
              <div class="param-control">
                <van-stepper v-model="ai.speedTwo" class="param-stepper" integer />
              </div>
            </div>
          </div>

          <div v-else-if="activeCategory === '料筒温度'" class="param-list">
            <div class="param-row">
              <span class="param-label">参数1</span>
              <div class="param-control">
                <van-stepper v-model="ai.barrel[activeBarrelZone].tempOne" class="param-stepper" integer />
              </div>
            </div>
            <div class="param-row">
              <span class="param-label">参数2</span>
              <div class="param-control">
                <van-stepper v-model="ai.barrel[activeBarrelZone].tempTwo" class="param-stepper" integer />
              </div>
            </div>
          </div>

          <div v-else class="param-empty">
            <p>{{ activeCategory }} 暂无可对比参数</p>
          </div>
        </div>
      </article>

      <article class="panel-card current-panel">
        <header class="panel-heading">当前工艺参数</header>
        <div class="panel-content">
          <div v-if="activeCategory === '注射速度'" class="param-list">
            <div class="param-row">
              <span class="param-label">参数1</span>
              <div class="param-control">
                <van-stepper v-model="current.speedOne" class="param-stepper" integer />
              </div>
            </div>
            <div class="param-row">
              <span class="param-label">参数2</span>
              <div class="param-control">
                <van-stepper v-model="current.speedTwo" class="param-stepper" integer />
              </div>
            </div>
          </div>

          <div v-else-if="activeCategory === '料筒温度'" class="param-list">
            <div class="param-row">
              <span class="param-label">参数1</span>
              <div class="param-control">
                <van-stepper v-model="current.barrel[activeBarrelZone].tempOne" class="param-stepper" integer />
              </div>
            </div>
            <div class="param-row">
              <span class="param-label">参数2</span>
              <div class="param-control">
                <van-stepper v-model="current.barrel[activeBarrelZone].tempTwo" class="param-stepper" integer />
              </div>
            </div>
          </div>

          <div v-else class="param-empty">
            <p>{{ activeCategory }} 当前暂无对应参数</p>
          </div>
        </div>
      </article>
    </section>

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
import { reactive, ref } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back'])

const categories = ['料筒温度', '注射速度', '注射压力', '保压', '冷却时间']
const barrelZones = ['前段', '中段', '后段', '喷嘴']
const activeCategory = ref('料筒温度')
const activeBarrelZone = ref('前段')

const ai = reactive({
  speedOne: 30,
  speedTwo: 25,
  barrel: {
    前段: { tempOne: 180, tempTwo: 185 },
    中段: { tempOne: 190, tempTwo: 195 },
    后段: { tempOne: 200, tempTwo: 205 },
    喷嘴: { tempOne: 210, tempTwo: 215 }
  }
})

const current = reactive({
  speedOne: 20,
  speedTwo: 20,
  barrel: {
    前段: { tempOne: 175, tempTwo: 180 },
    中段: { tempOne: 185, tempTwo: 190 },
    后段: { tempOne: 195, tempTwo: 200 },
    喷嘴: { tempOne: 205, tempTwo: 210 }
  }
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
  padding-bottom: 124px;
}

.hero-copy {
  display: grid;
  gap: 4px;
  padding: 2px 2px 0;
}

.page-title {
  margin: 0;
  color: #1f2420;
  font-size: clamp(20px, 6vw, 28px);
  font-weight: 700;
  line-height: 1.2;
  text-align: center;
}

.page-subtitle {
  margin: 0;
  color: rgba(31, 36, 32, 0.62);
  font-size: clamp(12px, 3.4vw, 14px);
  line-height: 1.45;
  text-align: center;
}

.category-tabs {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 6px;
}

.category-tab {
  min-width: 0;
  padding: 9px 8px;
  border: 1px solid rgba(105, 189, 0, 0.18);
  border-radius: 999px;
  color: rgba(31, 36, 32, 0.66);
  background: rgba(255, 255, 255, 0.8);
  font-size: clamp(12px, 3.4vw, 15px);
  font-weight: 500;
  line-height: 1.15;
  text-align: center;
  cursor: pointer;
  transition: all 0.15s ease;
}

.category-tab.active {
  color: #1f2420;
  font-weight: 700;
  border-color: rgba(105, 189, 0, 0.46);
  background: rgba(105, 189, 0, 0.1);
  box-shadow: 0 6px 16px rgba(105, 189, 0, 0.08);
}

.subtabs-card {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 6px;
  padding: 7px;
  border: 1px solid rgba(31, 36, 32, 0.08);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 10px 24px rgba(31, 36, 32, 0.06);
}

.subtab {
  min-width: 0;
  padding: 7px 6px;
  border: 1px solid rgba(105, 189, 0, 0.18);
  border-radius: 999px;
  color: rgba(31, 36, 32, 0.68);
  background: #fff;
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1.1;
  white-space: nowrap;
}

.subtab.active {
  color: #1f2420;
  font-weight: 700;
  background: rgba(105, 189, 0, 0.1);
  border-color: rgba(105, 189, 0, 0.42);
}

.comparison-stack {
  display: grid;
  gap: 10px;
}

.panel-card {
  display: flex;
  flex-direction: column;
  min-width: 0;
  border: 1px solid rgba(31, 36, 32, 0.08);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.92);
  overflow: hidden;
  box-shadow: 0 12px 28px rgba(31, 36, 32, 0.08);
}

.panel-heading {
  margin: 0;
  padding: 11px 12px;
  color: #1f2420;
  font-size: clamp(13px, 3.6vw, 16px);
  font-weight: 700;
  line-height: 1.2;
  text-align: left;
  border-bottom: 1px solid rgba(31, 36, 32, 0.08);
  background: rgba(105, 189, 0, 0.05);
}

.panel-content {
  flex: 1;
  padding: 12px;
}

.param-list {
  display: grid;
  gap: 10px;
}

.param-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 4px;
  flex-wrap: nowrap;
  min-width: 0;
}

.param-label {
  flex: 0 0 auto;
  min-width: 48px;
  flex-shrink: 0;
  color: #1f2420;
  font-size: clamp(14px, 4.2vw, 18px);
  font-weight: 600;
  line-height: 1.2;
  white-space: nowrap;
}

.param-control {
  flex: 0 0 auto;
  display: inline-flex;
  align-items: center;
  white-space: nowrap;
  min-width: 0;
}

.param-stepper {
  display: inline-flex;
  align-items: center;
  flex-wrap: nowrap;
  white-space: nowrap;
}

.param-stepper :deep(.van-stepper__minus),
.param-stepper :deep(.van-stepper__plus) {
  width: 24px;
  height: 24px;
  min-width: 24px;
  padding: 0;
  box-sizing: border-box;
  line-height: 1;
  font-size: 13px;
  font-weight: 700;
  text-align: center;
  border-radius: 8px;
  color: #fff;
  background: #9bd969;
  flex: 0 0 auto;
}

.param-stepper :deep(.van-stepper__input) {
  width: 42px;
  height: 24px;
  margin: 0 2px;
  padding: 0 2px;
  text-align: center;
  color: #1f2420;
  background: rgba(105, 189, 0, 0.08);
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  flex: 0 0 auto;
}

.param-empty {
  min-height: 110px;
  display: grid;
  place-items: center;
  color: rgba(31, 36, 32, 0.5);
  font-size: 14px;
  text-align: center;
  line-height: 1.5;
}

@media (min-width: 480px) {
  .category-tabs {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .page-title,
  .page-subtitle {
    text-align: left;
  }
}

@media (min-width: 640px) {
  .comparison-stack {
    grid-template-columns: 1fr 1fr;
  }

  .page-shell {
    max-width: 430px;
    margin: 0 auto;
    box-shadow: 0 0 0 1px rgba(31, 36, 32, 0.08);
  }
}
</style>
