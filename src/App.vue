<template>
  <div class="app">
    <ui-tooltip>
      <template #activator>
        <span>Show content</span>
      </template>
      <template #content>
        <div>Ui tooltip content</div>
      </template>
    </ui-tooltip>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import UiTooltip from '@/components/ui-tooltip/ui-tooltip.vue';

export default defineComponent({
  name: 'App',
  components: {
    UiTooltip,
  },
});
</script>

<style lang="scss">
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
