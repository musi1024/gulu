<template>
  <div class="tabs-item">
    <slot></slot>
  </div>
</template>
<script>
  export default {
    name: 'GuluTabsItem',
    inject: ['eventBus'],
    props: {
      disabled: {
        type: Boolean,
        default: false
      }
    },
     created () {
      this.eventBus.$on('update:selected', (name) => {
        
      })
    },
  }
</script>
<style>
  .tabs-item {
  }
</style> 