<template>
  <main class="page-shell setup3-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="setup3-hero">
      <h1 class="setup3-title">工艺参数设置</h1>
      <p class="setup3-subtitle">选择参数类别，调整具体数值</p>
    </section>

    <section class="parameter-layout" aria-label="参数设置">
      <aside class="parameter-tabs" aria-label="参数分类">
        <button
          v-for="item in categories"
          :key="item"
          class="parameter-tab"
          :class="{ active: activeCategory === item }"
          type="button"
          @click="activeCategory = item"
        >
          <span class="tab-icon" aria-hidden="true">
            <svg v-if="item === '料筒温度'" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 14.76V3.5a2.5 2.5 0 0 0-5 0v11.26a4.5 4.5 0 1 0 5 0z"/></svg>
            <svg v-else-if="item === '注射速度'" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
            <svg v-else-if="item === '注射压力'" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg>
            <svg v-else-if="item === '保压'" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
            <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 2v6h.01M6 10v2M6 14v2M6 18v2M6 22v-2M12 2v4M12 8v2M12 12v2M12 16v2M12 20v2M18 2v3M18 8v2M18 14v2M18 18v2"/><circle cx="6" cy="8" r="2"/><circle cx="12" cy="6" r="2"/><circle cx="18" cy="5" r="2"/></svg>
          </span>
          <span class="tab-text">{{ item }}</span>
        </button>
      </aside>

      <section class="parameter-content" aria-label="参数详情">
        <div class="parameter-content-header">
          <span class="parameter-content-icon" aria-hidden="true">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="3"/><path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"/></svg>
          </span>
          <span class="parameter-content-title">{{ activeCategory }}</span>
        </div>
        <div class="parameter-divider"></div>

        <div v-if="activeCategory === '注射速度'" class="parameter-list">
          <div class="parameter-card">
            <div class="parameter-card-header">
              <span class="parameter-card-icon">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="2" x2="12" y2="6"/><line x1="12" y1="18" x2="12" y2="22"/><line x1="4.93" y1="4.93" x2="7.76" y2="7.76"/><line x1="16.24" y1="16.24" x2="19.07" y2="19.07"/></svg>
              </span>
              <span class="parameter-card-name">参数1</span>
            </div>
            <van-stepper v-model="speedOne" class="parameter-stepper" integer />
          </div>
          <div class="parameter-card">
            <div class="parameter-card-header">
              <span class="parameter-card-icon">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="2" x2="12" y2="6"/><line x1="12" y1="18" x2="12" y2="22"/><line x1="4.93" y1="4.93" x2="7.76" y2="7.76"/><line x1="16.24" y1="16.24" x2="19.07" y2="19.07"/></svg>
              </span>
              <span class="parameter-card-name">参数2</span>
            </div>
            <van-stepper v-model="speedTwo" class="parameter-stepper" integer />
          </div>
        </div>

        <div v-else class="parameter-empty">
          <div class="empty-icon">
            <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
          </div>
          <p>{{ activeCategory }} 参数待设置</p>
        </div>
      </section>
    </section>

    <footer class="action-bar split-action-bar">
      <van-button class="action-button secondary-button" block @click="emit('back')">
        上一步
      </van-button>
      <van-button class="action-button confirm-button" block @click="confirmParams">
        确认
      </van-button>
    </footer>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back', 'next'])

const categories = ['料筒温度', '注射速度', '注射压力', '保压', '冷却时间']
const activeCategory = ref('注射速度')
const speedOne = ref(20)
const speedTwo = ref(20)

function confirmParams() {
  showToast('参数已确认')
  emit('next', {
    speedOne: speedOne.value,
    speedTwo: speedTwo.value,
    activeCategory: activeCategory.value
  })
}
</script>

<style scoped>
.setup3-page {
  display: flex;
  flex-direction: column;
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

.parameter-layout {
  display: grid;
  grid-template-columns: 96px minmax(0, 1fr);
  min-height: 480px;
  margin-top: 14px;
  border: 1px solid rgba(105, 189, 0, 0.16);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.78);
  box-shadow: 0 8px 28px rgba(105, 189, 0, 0.08);
  overflow: hidden;
}

.parameter-tabs {
  display: grid;
  align-content: start;
  border-right: 1px solid rgba(31, 36, 32, 0.1);
  background: rgba(105, 189, 0, 0.03);
}

.parameter-tab {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  min-width: 0;
  min-height: 72px;
  padding: 10px 6px;
  border: 0;
  border-bottom: 1px solid rgba(31, 36, 32, 0.06);
  color: rgba(31, 36, 32, 0.55);
  background: transparent;
  cursor: pointer;
  transition: color 0.15s ease, background 0.15s ease;
}

.parameter-tab:last-child {
  border-bottom: 0;
}

.parameter-tab:hover {
  background: rgba(105, 189, 0, 0.04);
}

.parameter-tab.active {
  color: #69bd00;
  background: rgba(105, 189, 0, 0.08);
  box-shadow: inset 3px 0 0 #69bd00;
}

.tab-icon {
  display: grid;
  place-items: center;
  flex: 0 0 auto;
}

.tab-text {
  font-size: clamp(11px, 3.2vw, 14px);
  font-weight: 500;
  line-height: 1.15;
  text-align: center;
}

.parameter-tab.active .tab-text {
  font-weight: 600;
}

.parameter-content {
  min-width: 0;
  padding: 16px 16px 20px;
}

.parameter-content-header {
  display: flex;
  align-items: center;
  gap: 6px;
}

.parameter-content-icon,
.parameter-card-icon {
  display: grid;
  place-items: center;
  color: rgba(105, 189, 0, 0.7);
}

.parameter-content-title {
  color: #1f2420;
  font-size: clamp(15px, 4.4vw, 20px);
  font-weight: 600;
  line-height: 1.2;
}

.parameter-divider {
  height: 1px;
  margin: 10px -16px 18px;
  background: linear-gradient(90deg, rgba(105, 189, 0, 0.3), transparent);
}

.parameter-list {
  display: grid;
  gap: 14px;
}

.parameter-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
  padding: 12px 14px;
  border: 1px solid rgba(105, 189, 0, 0.12);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.8);
  transition: box-shadow 0.15s ease;
}

.parameter-card:hover {
  box-shadow: 0 2px 10px rgba(105, 189, 0, 0.08);
}

.parameter-card-header {
  display: flex;
  align-items: center;
  gap: 6px;
  flex: 0 0 auto;
}

.parameter-card-name {
  color: #1f2420;
  font-size: clamp(14px, 4vw, 18px);
  font-weight: 500;
  line-height: 1;
}

.parameter-stepper :deep(.van-stepper__minus),
.parameter-stepper :deep(.van-stepper__plus) {
  width: 34px;
  height: 34px;
  border-radius: 8px;
  color: #fff;
  background: #9bd969;
}

.parameter-stepper :deep(.van-stepper__input) {
  width: 52px;
  height: 34px;
  margin: 0 6px;
  color: #1f2420;
  background: rgba(105, 189, 0, 0.06);
  border: 1px solid rgba(105, 189, 0, 0.15);
  border-radius: 8px;
  font-size: clamp(14px, 4.2vw, 20px);
  font-weight: 600;
}

.parameter-empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  min-height: 280px;
  color: rgba(31, 36, 32, 0.35);
  font-size: 15px;
  text-align: center;
}

.empty-icon {
  opacity: 0.5;
}

@media (max-width: 380px) {
  .parameter-layout {
    grid-template-columns: 78px minmax(0, 1fr);
  }

  .parameter-tab {
    min-height: 62px;
    padding: 8px 4px;
  }

  .tab-text {
    font-size: clamp(10px, 2.8vw, 12px);
  }

  .parameter-content {
    padding: 12px 12px 16px;
  }

  .parameter-card {
    padding: 10px 10px;
  }
}
</style>
