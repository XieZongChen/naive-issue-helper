<template>
  <n-card>
    <n-form
      ref="formRef"
      :model="form"
      label-width="auto"
      :rules="rules"
      label-position="top"
      size="large"
      class="form"
    >
      <n-grid cols="2" :x-gap="16">
        <!-- 选择要提交的库 -->
        <n-form-item-gi :label="contentText.repoSelectHint" path="repo">
          <n-select
            placeholder="请选择"
            v-model:value="form.repo"
            :options="repoOptions"
            class="form-select"
          />
        </n-form-item-gi>

        <!-- Issue 类别 -->

        <n-form-item-gi :label="contentText.issueTypesHint" path="type">
          <n-select
            placeholder="请选择"
            v-model:value="form.type"
            :options="issueTypeOptions"
            class="form-select"
          />
        </n-form-item-gi>
      </n-grid>

      <!-- Issue 标题 -->
      <n-form-item :label="contentText.issueTitleHint" path="title">
        <n-input v-model:value="form.title" placeholder="请输入" />
      </n-form-item>

      <template v-if="isBug">
        <n-grid cols="2" :x-gap="16">
          <!-- 项目版本 -->
          <n-form-item-gi
            :label="contentText.versionRepositoryHint"
            path="versionRepository"
          >
            <n-select
              placeholder="请选择"
              v-model:value="form.versionRepository"
              :options="version.repo"
              class="form-select"
            />
          </n-form-item-gi>

          <!-- 环境 -->
          <n-form-item-gi
            :label="contentText.versionBrowserHint"
            path="versionBrowser"
          >
            <n-input v-model:value="form.versionBrowser" placeholder="请输入" />
          </n-form-item-gi>
        </n-grid>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{ contentText.firstTip }}</span>
        </n-alert>

        <!-- 重现链接 -->
        <n-form-item :label="contentText.reproduceHint" path="reproduce">
          <n-input v-model:value="form.reproduce" placeholder="请输入" />
        </n-form-item>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{ contentText.secondTip }}</span></n-alert
        >
        <n-button
          text
          type="primary"
          v-html="contentText.reproduceHintSamll"
          @click="tipVisible = true"
          class="m-b-24"
        />
        <n-modal v-model:show="tipVisible">
          <n-card
            :title="contentText.reproduceTitle"
            closable
            @close="tipVisible = false"
            style="width: 50%"
          >
            <p v-html="contentText.reproduceExplain"></p>
          </n-card>
        </n-modal>

        <!-- 重现步骤 -->
        <n-form-item :label="contentText.stepsHint" path="steps">
          <n-input
            v-model="form.steps"
            type="textarea"
            placeholder="请输入"
            :autosize="{ minRows: 2, maxRows: 5 }"
          />
        </n-form-item>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{ contentText.thirdTip }}</span></n-alert
        >

        <!-- 期望的结果是什么 -->
        <n-form-item :label="contentText.expectHint" path="expected">
          <n-input
            v-model="form.expected"
            type="textarea"
            placeholder="请输入"
            :autosize="{ minRows: 2, maxRows: 5 }"
          ></n-input>
        </n-form-item>

        <!-- 实际的结果是什么 -->
        <n-form-item :label="contentText.actualHint" path="actual">
          <n-input
            v-model="form.actual"
            type="textarea"
            placeholder="请输入"
            :autosize="{ minRows: 2, maxRows: 5 }"
          ></n-input>
        </n-form-item>

        <!-- 补充说明（可选） -->
        <n-form-item :label="contentText.remarks">
          <n-input
            v-model="form.remarks"
            type="textarea"
            placeholder="请输入"
            :autosize="{ minRows: 2, maxRows: 5 }"
          ></n-input>
        </n-form-item>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{ contentText.fourthTip }}</span></n-alert
        >
      </template>

      <template v-else>
        <!-- 这个功能解决了什么问题 -->
        <n-form-item
          :label="contentText.functionContent"
          path="functionContent"
        >
          <n-input v-model="form.functionContent" placeholder="请输入" />
        </n-form-item>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{
            contentText.functionContentTip
          }}</span></n-alert
        >

        <!-- 你期望的 API 是怎样的 -->
        <n-form-item
          :label="contentText.functionalExpectations"
          path="functionalExpectations"
        >
          <n-input
            v-model="form.functionalExpectations"
            type="textarea"
            placeholder="请输入"
            :autosize="{ minRows: 2, maxRows: 5 }"
          ></n-input>
        </n-form-item>
        <n-alert type="default" class="m-b-24" show-icon>
          <template #icon>
            <n-icon :depth="3">
              <information-circle-outline />
            </n-icon>
          </template>
          <span class="alert-font">{{
            contentText.functionalExpectationsTip
          }}</span></n-alert
        >
      </template>

      <n-form-item class="preview">
        <n-button
          type="primary"
          v-html="contentText.preview"
          @click="handlePreview()"
        />
      </n-form-item>
    </n-form>
  </n-card>

  <n-modal v-model:show="previewVisible">
    <n-card
      :title="contentText.dialog.title"
      closable
      @close="tipVisible = false"
      style="width: 50%"
    >
      <div v-html="issueHTML"></div>
      <template #action>
        <n-button
          type="primary"
          v-html="contentText.dialog.button"
          @click="create()"
        />
      </template>
    </n-card>
  </n-modal>
</template>

<script lang="ts">
import {
  ref,
  defineComponent,
  Ref,
  reactive,
  toRefs,
  computed,
  watch,
  onMounted,
} from 'vue';
import {
  NForm,
  NFormItem,
  NFormItemGi,
  NGrid,
  NSelect,
  NInput,
  NAlert,
  NButton,
  NIcon,
  NModal,
  NCard,
} from 'naive-ui';
import { InformationCircleOutline } from '@vicons/ionicons5';
import axios from 'axios';
import marked from 'marked';
import hanabi from 'hanabi';
import * as content from '../content.json';
import { FormData, RepoItem } from '../data';

marked.setOptions({
  renderer: new marked.Renderer(),
  highlight: (code: any) => hanabi(code),
  pedantic: false,
  gfm: true,
  breaks: false,
  sanitize: false,
  smartLists: true,
  smartypants: false,
  xhtml: false,
});

const comment = '<!-- generated by issue-helper DO NOT REMOVE -->';

export default defineComponent({
  name: 'IssueForm',
  components: {
    NForm,
    NFormItem,
    NFormItemGi,
    NGrid,
    NSelect,
    NInput,
    NAlert,
    NButton,
    NIcon,
    NModal,
    NCard,
    InformationCircleOutline,
  },
  setup: () => {
    const contentText: Ref<any> = ref(content);
    const formRef: Ref<any> = ref(null);
    const formData: FormData = reactive({
      form: {
        repo: 'TuSimple/naive-ui',
        title: '',
        type: 'Bug',
        versionRepository: '',
        versionBrowser: '',
        reproduce: '',
        steps: '',
        expected: '',
        actual: '',
        remarks: '',
        functionContent: '',
        functionalExpectations: '',
      },
      repoOptions: contentText.value.repos.map((i) => {
        return { label: i.name, value: i.github };
      }),
      issueTypeOptions: contentText.value.issueTypes,
      version: {
        repo: [],
      },
    });
    const issue: Ref<string> = ref('');
    const issueHTML = computed(() => marked(issue.value));
    const tipVisible: Ref<boolean> = ref(false);
    const previewVisible: Ref<boolean> = ref(false);

    const isBug = computed(() => formData.form.type === 'Bug');

    const rules = computed(() => {
      const valid = contentText.value.valid;
      let rules: any = {};
      for (let prop in valid) {
        if (prop === 'remarks') {
          continue;
        }
        rules[prop] = [
          {
            required: true,
            message: valid[prop],
            trigger: ['blur', 'change'],
          },
        ];
      }

      rules['reproduce'].push({
        trigger: ['blur', 'change'],
        message: '请填写正确的重现链接',
        validator: (rule: any, val: string) => {
          return new Promise((resolve, reject) => {
            if (
              val &&
              !/(github|jsfiddle|codepen|jsbin|codesandbox)/gi.test(val)
            ) {
              reject(Error('请填写正确的重现链接'));
            } else {
              resolve('');
            }
          });
        },
      });
      console.log(rules);

      return rules;
    });

    const versionApi = computed(() => {
      const selectRepo = contentText.value.repos.find(
        (i) => i.github === formData.form.repo
      );
      return {
        repositoryVersion: `https://registry.npm.taobao.org/${
          selectRepo.npm && selectRepo.npm
        }`,
        vueVersion: 'https://registry.npm.taobao.org/vue',
      };
    });

    async function fetchRepositoryVersion() {
      const res = await axios.get(versionApi.value.repositoryVersion);
      formData.version.repo = Object.keys(res.data.versions).map((i) => {
        return { label: i, value: i };
      });
      formData.form.versionRepository = formData.version.repo[0].value;
    }
    onMounted(() => {
      fetchRepositoryVersion();
    });
    watch(
      () => formData.form.repo,
      () => {
        fetchRepositoryVersion();
      }
    );

    function createIssue() {
      issue.value = isBug.value
        ? `
### ${formData.form.repo} 项目版本
${formData.form.versionRepository}

### 环境
${formData.form.versionBrowser}

### 重现链接
${formData.form.reproduce}

### 重现步骤
${formData.form.steps}

### 期望的结果
${formData.form.expected}

### 实际的结果
${formData.form.actual}

### 补充说明
${formData.form.remarks}

`.trim()
        : `

### 这个功能解决的问题
${formData.form.functionContent}

### 期望的 API
${formData.form.functionalExpectations}

`.trim();
      previewVisible.value = true;
    }

    function handlePreview() {
      formRef.value.validate((errors: boolean) => {
        if (!errors) {
          createIssue();
        } else {
          console.error('error submit!!');
          return false;
        }
      });
    }

    const body = computed(() => {
      const issueString = `
${comment}

${issue.value}

${comment}
`;
      return encodeURIComponent(issueString).replace(/%2B/gi, '+');
    });

    function create() {
      const idx = contentText.value.repos.findIndex(
        (i: RepoItem) => i.npm === formData.form.repo
      );

      window.open(
        `https://github.com/${contentText.value.repos[idx].github}/issues/new?title=${formData.form.title}&body=${body.value}`
      );
    }
    return {
      ...toRefs(formData),
      contentText,
      formRef,
      rules,
      tipVisible,
      isBug,
      previewVisible,
      handlePreview,
      issueHTML,
      create,
    };
  },
});
</script>

<style scoped>
:deep(.n-form-item-label) {
  font-size: 18px;
}

.form {
  margin-top: 10px;
}

.alert-font {
  font-size: 13px;
  color: #909399;
}

.form-select {
  width: 100%;
}

.m-b-24 {
  margin-bottom: 24px;
}

.preview {
  margin-bottom: 40px;
  display: flex;
  justify-content: center;
}
</style>
