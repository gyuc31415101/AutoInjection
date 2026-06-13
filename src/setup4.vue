<template>
  <main class="page-shell defect-page">
    <header class="brand-header">
      <img class="brand-logo" src="/logo.png" alt="东华机械 Welltec" />
    </header>

    <section class="defect-panel" aria-label="不良类型选择">
      <div class="defect-hero">
        <h1 class="defect-title">不良类型</h1>
        <p class="defect-subtitle">
          勾选本次出现的缺陷并设定等级
          <span v-if="selectedCount > 0" class="defect-badge">已选 {{ selectedCount }} 项</span>
        </p>
      </div>
      <div class="defect-divider"></div>

      <div class="defect-list">
        <div
          v-for="item in defects"
          :key="item.name"
          class="defect-item"
          :class="{ expanded: item.checked }"
        >
          <label class="defect-label">
            <span class="defect-checkmark" :class="{ on: item.checked }">
              <svg v-if="item.checked" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="3"><path d="M20 6L9 17l-5-5"/></svg>
            </span>
            <input v-model="item.checked" type="checkbox" class="defect-input" />
            <span class="defect-name">{{ item.name }}</span>
          </label>

          <div v-if="item.checked" class="defect-controls">
            <span class="grade-label">等级</span>
            <div class="grade-chips">
              <button
                v-for="lv in ['轻微', '中等', '严重']"
                :key="lv"
                class="grade-chip"
                :class="{ active: item.level === lv }"
                type="button"
                @click="item.level = lv; syncPercentByLevel(item)"
              >
                {{ lv }}
              </button>
            </div>
            <div class="percent-group">
              <van-stepper
                v-model="item.percent"
                class="defect-stepper"
                integer
                :min="0"
                :max="100"
                :step="1"
              />
              <span class="percent-unit">%</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div v-if="isGenerating" class="ai-loading-mask" aria-live="polite">
      <div class="ai-loading-panel">
        <div class="ai-spinner" aria-hidden="true"></div>
        <p class="ai-loading-text">正在AI生成工艺参数</p>
      </div>
    </div>

    <footer class="action-bar split-action-bar">
      <van-button class="action-button secondary-button" block @click="emit('back')">
        上一步
      </van-button>
      <van-button
        class="action-button confirm-button"
        block
        :loading="isGenerating"
        :disabled="isGenerating"
        @click="submitDefects"
      >
        确认
      </van-button>
    </footer>
  </main>
</template>

<script setup>
import { computed, reactive, ref, onBeforeUnmount } from 'vue'
import { showToast } from 'vant'

const emit = defineEmits(['back', 'next'])

const defectNames = [
  '变形',
  '困气',
  '尺寸偏大',
  '尺寸偏小',
  '料花',
  '气纹',
  '水波纹',
  '浇口印',
  '烧焦',
  '熔接纹',
  '短射',
  '缩水',
  '脱模不良',
  '色差',
  '阴阳面',
  '顶白',
  '飞边'
]

const defects = reactive(
  defectNames.map((name) => ({
    name,
    checked: false,
    level: '轻微',
    percent: 20
  }))
)

const selectedCount = computed(() => defects.filter((d) => d.checked).length)

const isGenerating = ref(false)
let generateTimer = null

onBeforeUnmount(() => {
  if (generateTimer) {
    window.clearTimeout(generateTimer)
  }
})

function submitDefects() {
  if (isGenerating.value) {
    return
  }

  isGenerating.value = true
  generateTimer = window.setTimeout(() => {
    emit('next')
    isGenerating.value = false
  }, 5000)
}

function syncPercentByLevel(item) {
  const levelPercentMap = {
    轻微: 20,
    中等: 50,
    严重: 80
  }

  item.percent = levelPercentMap[item.level]
}
</script>

<style scoped>
.defect-page {
  display: flex;
  flex-direction: column;
}

.defect-panel {
  margin-top: 34px;
  padding: 0 2px;
  padding-bottom: 100px;
}

.defect-hero {
  text-align: center;
}

.defect-title {
  margin: 0;
  color: #1f2420;
  font-size: clamp(24px, 7vw, 34px);
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: 0;
}

.defect-subtitle {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  flex-wrap: wrap;
  margin: 4px 0 0;
  color: rgba(31, 36, 32, 0.52);
  font-size: clamp(13px, 3.8vw, 16px);
  font-weight: 400;
  line-height: 1.3;
}

.defect-badge {
  display: inline-flex;
  align-items: center;
  padding: 2px 10px;
  border-radius: 999px;
  color: #69bd00;
  background: rgba(105, 189, 0, 0.1);
  font-size: clamp(11px, 3.2vw, 13px);
  font-weight: 600;
  line-height: 1.6;
}

.defect-divider {
  height: 1px;
  margin: 10px -18px 14px;
  background: linear-gradient(90deg, transparent, rgba(105, 189, 0, 0.35), transparent);
}

.defect-list {
  display: grid;
  gap: 2px;
}

.defect-item {
  border-radius: 8px;
  transition: background 0.2s ease, box-shadow 0.2s ease;
  overflow: hidden;
}

.defect-item.expanded {
  background: rgba(105, 189, 0, 0.04);
  box-shadow: 0 1px 6px rgba(31, 36, 32, 0.05);
}

.defect-label {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 6px;
  cursor: pointer;
  user-select: none;
}

.defect-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.defect-checkmark {
  display: grid;
  place-items: center;
  width: 22px;
  height: 22px;
  flex: 0 0 auto;
  border: 2px solid rgba(31, 36, 32, 0.22);
  border-radius: 4px;
  transition: all 0.15s ease;
}

.defect-checkmark.on {
  border-color: #69bd00;
  background: #69bd00;
}

.defect-name {
  color: #1f2420;
  font-size: clamp(17px, 5vw, 22px);
  font-weight: 500;
  line-height: 1.15;
}

.defect-item.expanded .defect-name {
  font-weight: 600;
}

.defect-controls {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  min-width: 0;
  padding: 0 8px 10px 36px;
}

.grade-label {
  flex: 0 0 auto;
  color: rgba(31, 36, 32, 0.45);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1;
}

.grade-chips {
  display: flex;
  flex: 0 0 auto;
  gap: 4px;
}

.grade-chip {
  padding: 5px 12px;
  border: 1px solid rgba(31, 36, 32, 0.14);
  border-radius: 999px;
  color: rgba(31, 36, 32, 0.52);
  background: rgba(255, 255, 255, 0.8);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1;
  cursor: pointer;
  transition: all 0.15s ease;
}

.grade-chip:hover {
  border-color: rgba(105, 189, 0, 0.35);
  color: #1f2420;
}

.grade-chip.active {
  border-color: #69bd00;
  color: #fff;
  background: #69bd00;
  font-weight: 600;
}

.percent-group {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  flex: 0 0 auto;
  margin-left: auto;
}

.percent-unit {
  color: rgba(31, 36, 32, 0.45);
  font-size: clamp(12px, 3.4vw, 14px);
  font-weight: 500;
  line-height: 1;
}

.defect-stepper :deep(.van-stepper__minus),
.defect-stepper :deep(.van-stepper__plus) {
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
}

.defect-stepper :deep(.van-stepper__input) {
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

@media (max-width: 380px) {
  .defect-name {
    font-size: 16px;
  }

  .defect-controls {
    gap: 5px;
    padding: 0 6px 8px 30px;
  }

  .grade-chip {
    padding: 4px 9px;
    font-size: 11px;
  }

  .defect-stepper :deep(.van-stepper__minus),
  .defect-stepper :deep(.van-stepper__plus) {
    width: 22px;
    height: 22px;
    min-width: 22px;
  }

  .defect-stepper :deep(.van-stepper__input) {
    width: 32px;
    height: 22px;
  }
}
</style>
