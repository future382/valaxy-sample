<script lang="ts" setup>
import YunFooter from "valaxy-theme-yun/components/YunFooter.vue";
</script>

<template>
  <YunFooter>
    <VCLiveTime start-time="2021-09-20">
      <template #live-time-before>
        <span>风雨交加，已经在网络的海洋上航行了</span>
      </template>
      <template #live-time-after>
        <span>(●'◡'●)</span>
      </template>
    </VCLiveTime>
  </YunFooter>
</template>

<style>
@media (max-width: 1000px) {
  .va-footer {
    margin-bottom: 125px;
  }
}
</style>
