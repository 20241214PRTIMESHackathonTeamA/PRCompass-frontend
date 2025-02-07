<script setup lang="ts">
defineOptions({
  name: 'JudgeResult',
})

defineProps({
  label: {
    type: String,
    required: true,
  },
  result: {
    type: Object,
    required: false,
    default: null, // 判定結果オブジェクト
  },
  loading: {
    type: Boolean,
    required: true, // ローディング状態を親コンポーネントから受け取る
  },
})

import { ref } from 'vue'
import checkmarkImage from '../../assets/icon_checkmark.png'
import crossmarkImage from '../../assets/icon_crossmark.png'
import dropdownImage from '../../assets/icon_nav_under_arrow.png'
import loadingImage from '../../assets/loading.svg'

const showDetails = ref(false)

function toggleDetails() {
  showDetails.value = !showDetails.value
}
</script>

<template>
  <div class="container">
    <!-- 白い四角 -->
    <div class="rectangle">

      <!-- チェックマークまたはバツマーク、ローディング表示 -->
      <div class="icon">
        <img v-if="loading" :src="loadingImage" class="loading-mark-img" alt="Loading Icon" loading="eager" />
        <!-- ローディングスピナー -->
        <img v-else
          :src="result?.valid ? checkmarkImage : crossmarkImage"
          :class="result?.valid ? 'check-mark-img': 'cross-mark-img'"
          alt="Result Icon" />
      </div>
      <!-- テキスト -->
      <span class="text">{{ label }}</span>
      <!-- 下マークボタン -->
      <button class="dropdown-button" @click="toggleDetails" :disabled="loading">
        <img class="dropdown-icon" :src="dropdownImage" alt="Dropdown Icon" />
      </button>
    </div>
    <!-- 詳細表示 -->
    <div :class="['details', { show: showDetails }]">
      <p>{{ result?.reason }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  margin-bottom: 20px;
  width: 588px;
}
/* 白い四角 */
.rectangle {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 76px;
  background: var(--color-white);
  border-radius: 7px;
  padding-left: 20px;
  padding-right: 10px;
}

.icon .check-mark-img,
.icon .cross-mark-img,
.icon .loading-mark-img
{
  margin-right: 20px;
}
.icon .check-mark-img {
  width: 47px;
  height: 35px;
}
.icon .cross-mark-img {
  width: 30px;
  height: 30px;
}
.icon .loading-mark-img {
  width: 30px;
  height: 30px;
}

/* テキストの位置 */
.text {
  width: 100%;
  font-size: 16px;
  color: #000;
  margin-left: 0; /* 左寄せ */
  text-align: left; /* テキストを左寄せ */
  font-weight: bold;
}

/* ボタンのスタイル */
.dropdown-button {
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  width: 45px; /* ボタンの明確なサイズを指定 */
  height: 45px; /* ボタンの明確なサイズを指定 */
  transition: background-color 0.3s ease;
  border-radius: 50%; /* 丸いボタンに */
}

.dropdown-button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.dropdown-button:hover {
  background-color: #f0f0f0;
}

/* ボタン内のアイコン */
.dropdown-icon {
  width: 26px; /* アイコンのサイズを固定 */
  height: 13px;
}

/* 詳細表示（灰色スペース） */
.details {
  width: 100%;
  background: #ebebeb;
  border-radius: 6px;
  padding: 10px;
  font-size: 14px;
  color: #000;
  transition:
    max-height 0.3s ease,
    opacity 0.3s ease;
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  line-height: 1.8;
  padding-left: 35px;
  text-align: left;
}

.details.show {
  max-height: 200px; /* 最大高さを適宜設定 */
  opacity: 1;
}
</style>
