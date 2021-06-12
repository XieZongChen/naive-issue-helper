<template>
  <PageHead />
  <div class="content-box">
    <div class="content"><Intro /></div>
    <div class="content"><IssueForm /></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import PageHead from './PageHead.vue';
import Intro from './Intro.vue';
import IssueForm from './IssueForm.vue';

export default defineComponent({
  name: 'IssuePage',
  components: {
    PageHead,
    Intro,
    IssueForm,
  },
});
</script>

<style scoped>
.content-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #f0f2f5;
}

.content {
  width: 50%;
  margin-top: 40px;
}
</style>
