<template>
  <n-card :title="contentText.introTitle">
    <p v-html="contentText.introOne" />

    <n-button
      text
      type="primary"
      v-html="contentText.explainTitle"
      @click="introVisible = true"
    />

    <p v-html="contentText.introTwo" />

    <n-modal v-model:show="introVisible">
      <n-card
        :title="contentText.explainTitle"
        closable
        @close="introVisible = false"
        style="width: 50%"
      >
        <p v-html="contentText.explain"></p>
      </n-card>
    </n-modal>
  </n-card>
</template>

<script lang="ts">
import { ref, defineComponent, Ref } from 'vue';
import { NCard, NButton, NModal } from 'naive-ui';
import * as content from '../content.json';

export default defineComponent({
  name: 'Intro',
  components: {
    NCard,
    NButton,
    NModal,
  },
  setup: () => {
    const contentText: Ref<any> = ref(content);
    const introVisible: Ref<boolean> = ref(false);
    return { contentText, introVisible };
  },
});
</script>

<style scoped></style>
