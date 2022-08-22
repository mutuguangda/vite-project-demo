<script setup lang="ts">
import { onMounted, ref } from "vue";

// 声明组件props
defineProps<{
  width: string
}>()

// tooltip 位置自适应
const getPopupContainer = (trigger: HTMLElement) => {
  return trigger.parentElement;
};

const slotWrapper = ref<HTMLElement | null>(null);
const tooltipWrapper = ref<HTMLElement | null>(null);
const isTooltipShow = ref<boolean>(true)
// slot文本
const slotText = ref<string>("");

onMounted(() => {
  const slotDOM = slotWrapper.value!;
  const tooltipDOM = tooltipWrapper.value!;
  isTooltipShow.value = tooltipDOM.offsetWidth < slotDOM.offsetWidth
  slotText.value = slotDOM.innerText;
});
</script>

<template>
  <div class="tooltip-container text-ellipsis" ref="tooltipWrapper" :style="{ 'width': width + 'px' }" >
    <a-tooltip arrow-point-at-center :get-popup-container="getPopupContainer">
      <template v-if="isTooltipShow" #title>
        <span>{{ slotText }}</span>
      </template>
      <span ref="slotWrapper">
        <slot></slot>
      </span>
    </a-tooltip>
  </div>
</template>

<style lang="less" scoped>
.text-ellipsis {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>
