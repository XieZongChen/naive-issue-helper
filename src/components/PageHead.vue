<template>
  <n-layout-header bordered class="nav">
    <div class="nav-box">
      <n-text tag="div" class="ui-logo" :depth="1">
        <img src="../assets/logo.svg" />
        <span>Naive UI Issue-helper</span>
      </n-text>
      <n-button text class="nav-picker" disabled> English</n-button>
    </div>
  </n-layout-header>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { NMenu, NLayoutHeader, NText, NButton } from 'naive-ui';

export default defineComponent({
  name: 'PageHead',
  components: {
    NLayoutHeader,
    NText,
    NMenu,
    NButton,
  }
});
</script>

<style scoped>
.nav {
  height: calc(var(--header-height) - 1px);
  display: flex;
  justify-content: center;
}

.nav-box {
  width: 50%;
  display: flex;
  justify-content: space-between;
}

.ui-logo {
  cursor: pointer;
  display: flex;
  align-items: center;
  font-size: 18px;
}

.ui-logo > img {
  margin-right: 12px;
  height: 32px;
  width: 32px;
}
</style>
