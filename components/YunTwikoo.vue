<!-- YunTwikoo.vue -->
<script lang="ts" setup>
import { useTwikooWithOptions } from 'valaxy-addon-twikoo'

// twikoo loading style patch
import 'valaxy-addon-twikoo/client/styles/index.scss'

useTwikooWithOptions()
</script>

<template>
  <div id="tcomment" w="full" />
</template>

<style lang="scss">
// custom twikoo style
</style>
