<template>
  <main class="page-shell setup2-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="setup2-hero">
      <h1 class="setup2-title">产品信息</h1>
      <p class="setup2-subtitle">完善产品参数，AI 将据此生成优化工艺</p>
    </section>

    <section class="setup2-form">
      <div class="form-card">
        <div class="form-card-header">
          <span class="form-card-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="M21 15l-5-5L5 21"/></svg>
          </span>
          <span class="form-card-label">产品图片</span>
        </div>
        <van-uploader
          v-model="productImages"
          class="setup2-uploader"
          accept="image/*"
          multiple
          :max-count="3"
        />
      </div>

      <div class="form-card">
        <div class="form-card-header">
          <span class="form-card-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><path d="M14 2v6h6"/></svg>
          </span>
          <span class="form-card-label">材料类型</span>
        </div>
        <div class="material-chips">
          <button
            v-for="m in ['PP', 'ABS', 'OPS', 'PE']"
            :key="m"
            class="material-chip"
            :class="{ active: material === m }"
            type="button"
            @click="material = m"
          >
            {{ m }}
          </button>
        </div>
      </div>

      <div class="form-card">
        <div class="form-card-header">
          <span class="form-card-icon">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 20V10M18 20V4M6 20v-4"/></svg>
          </span>
          <span class="form-card-label">规格参数</span>
        </div>
        <div class="spec-grid">
          <div class="spec-item">
            <span class="spec-name">产品单重</span>
            <div class="spec-control">
              <van-stepper v-model="totalWeight" class="setup2-stepper" integer />
              <span class="spec-unit">g</span>
            </div>
          </div>
          <div class="spec-item">
            <span class="spec-name">壁厚</span>
            <div class="spec-control">
              <van-stepper v-model="wallThickness" class="setup2-stepper" integer />
              <span class="spec-unit">mm</span>
            </div>
          </div>
          <div class="spec-item">
            <span class="spec-name">模具穴数</span>
            <div class="spec-control">
              <van-stepper v-model="cavityCount" class="setup2-stepper" integer />
              <span class="spec-unit">pcs</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <footer class="action-bar split-action-bar">
      <van-button class="action-button secondary-button" block @click="emit('back')">
        取消
      </van-button>
      <van-button class="action-button confirm-button" block @click="confirmSetup">
        确认
      </van-button>
    </footer>

    <div v-if="showLoading" class="ai-loading-mask" aria-live="polite">
      <div class="ai-loading-panel">
        <div class="ai-spinner" aria-hidden="true"></div>
        <p class="ai-loading-text">正在AI生成工艺参数</p>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, onBeforeUnmount } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back', 'next'])

const productImages = ref([])
const material = ref('PP')
const totalWeight = ref(50)
const wallThickness = ref(50)
const cavityCount = ref(20)
const showLoading = ref(false)
let loadingTimer = null

function confirmSetup() {
  showLoading.value = true
  loadingTimer = window.setTimeout(() => {
    showLoading.value = false
    emit('next', {
      material: material.value,
      totalWeight: totalWeight.value,
      wallThickness: wallThickness.value,
      cavityCount: cavityCount.value
    })
  }, 10000)
}

onBeforeUnmount(() => {
  if (loadingTimer) {
    window.clearTimeout(loadingTimer)
  }
})
</script>

<style scoped>
.setup2-page {
  display: flex;
  flex-direction: column;
  padding-bottom: 120px;
}

.setup2-hero {
  padding: 4px 4px 0;
}

.setup2-title {
  margin: 0;
  color: #1f2420;
  font-size: clamp(22px, 6vw, 30px);
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: 0;
  text-align: center;
}

.setup2-subtitle {
  margin: 4px 0 0;
  color: rgba(31, 36, 32, 0.5);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 400;
  line-height: 1.4;
  text-align: center;
}

.setup2-form {
  display: grid;
  gap: 12px;
  margin-top: 14px;
}

.form-card {
  padding: 16px;
  border: 1px solid rgba(105, 189, 0, 0.14);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.78);
  box-shadow: 0 4px 16px rgba(105, 189, 0, 0.06);
}

.form-card-header {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 12px;
}

.form-card-icon {
  display: grid;
  place-items: center;
  color: rgba(105, 189, 0, 0.7);
}

.form-card-label {
  color: #1f2420;
  font-size: clamp(15px, 4.2vw, 19px);
  font-weight: 600;
  line-height: 1;
}

.setup2-uploader {
  min-width: 0;
}

.setup2-uploader :deep(.van-uploader__wrapper) {
  display: grid;
  grid-template-columns: repeat(3, 64px);
  gap: 8px;
}

.setup2-uploader :deep(.van-uploader__upload),
.setup2-uploader :deep(.van-uploader__preview),
.setup2-uploader :deep(.van-uploader__preview-image) {
  width: 64px;
  height: 64px;
  margin: 0;
  border-radius: 8px;
}

.setup2-uploader :deep(.van-uploader__upload) {
  border: 1px dashed rgba(105, 189, 0, 0.5);
  color: #69bd00;
  background: rgba(105, 189, 0, 0.05);
}

.material-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.material-chip {
  flex: 0 0 auto;
  min-width: 58px;
  padding: 8px 16px;
  border: 1px solid rgba(31, 36, 32, 0.14);
  border-radius: 999px;
  color: rgba(31, 36, 32, 0.6);
  background: rgba(255, 255, 255, 0.8);
  font-size: clamp(14px, 4vw, 18px);
  font-weight: 500;
  line-height: 1;
  cursor: pointer;
  transition: all 0.15s ease;
}

.material-chip:hover {
  border-color: rgba(105, 189, 0, 0.3);
  color: #1f2420;
}

.material-chip.active {
  border-color: #69bd00;
  color: #fff;
  background: #69bd00;
  font-weight: 600;
  box-shadow: 0 3px 10px rgba(105, 189, 0, 0.25);
}

.spec-grid {
  display: grid;
  gap: 14px;
}

.spec-item {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 8px;
  min-width: 0;
}

.spec-name {
  flex: 0 0 auto;
  color: rgba(31, 36, 32, 0.72);
  font-size: clamp(14px, 4vw, 18px);
  font-weight: 500;
  line-height: 1;
  width: 7em;
  white-space: nowrap;
}

.spec-control {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  flex: 0 0 auto;
  white-space: nowrap;
}

.spec-unit {
  color: rgba(31, 36, 32, 0.5);
  font-size: clamp(13px, 3.6vw, 16px);
  font-weight: 500;
  line-height: 1;
  white-space: nowrap;
}

.setup2-stepper :deep(.van-stepper__minus),
.setup2-stepper :deep(.van-stepper__plus) {
  width: 34px;
  height: 34px;
  border-radius: 8px;
  color: #fff;
  background: #9bd969;
}

.setup2-stepper :deep(.van-stepper__input) {
  width: 54px;
  height: 34px;
  margin: 0 6px;
  color: #1f2420;
  background: rgba(105, 189, 0, 0.06);
  border: 1px solid rgba(105, 189, 0, 0.15);
  border-radius: 8px;
  font-size: clamp(14px, 4.2vw, 18px);
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
  to {
    transform: rotate(360deg);
  }
}
</style>
