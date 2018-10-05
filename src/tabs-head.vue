<template>
  <div class="tabs-head">
    <slot></slot>
    <slot name="actions"></slot>
  </div>
</template>
<script>
  export default {
    name: 'GuluTabsHead',
    inject: ['eventBus'],
    created () {
      this.$emit('update:selected', 'tabs-head 抛出的数据')
    }
  }
</script>
<style>
  .tabs-head {
  }
</style> 