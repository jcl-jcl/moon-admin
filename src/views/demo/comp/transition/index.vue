<template>
  <PageWrapper title="动画组件示例">
    <div class="flex">
      <Select
        v-model:value="value"
        :options="options"
        placeholder="选择动画"
        :style="{ width: '150px' }"
      />
      <a-button type="primary" class="ml-4" @click="start"> start </a-button>
    </div>
    <component :is="`${value}Transition`">
      <div v-show="show" class="box"></div>
    </component>
  </PageWrapper>
</template>
<script lang="ts">
  import { Select } from 'ant-design-vue';
  import { defineComponent, ref } from 'vue';

  import { PageWrapper } from '@/components/Page';
  import {
    ExpandTransition,
    ExpandXTransition,
    FadeTransition,
    ScaleRotateTransition,
    ScaleTransition,
    ScrollXReverseTransition,
    ScrollXTransition,
    ScrollYReverseTransition,
    ScrollYTransition,
    SlideXReverseTransition,
    SlideXTransition,
    SlideYReverseTransition,
    SlideYTransition,
  } from '@/components/Transition';

  const transitionList = [
    'Fade',
    'Scale',
    'SlideY',
    'ScrollY',
    'SlideYReverse',
    'ScrollYReverse',
    'SlideX',
    'ScrollX',
    'SlideXReverse',
    'ScrollXReverse',
    'ScaleRotate',
    'ExpandX',
    'Expand',
  ];
  const options = transitionList.map((item) => ({
    label: item,
    value: item,
    key: item,
  }));

  export default defineComponent({
    components: {
      Select,
      PageWrapper,
      FadeTransition,
      ScaleTransition,
      SlideYTransition,
      ScrollYTransition,
      SlideYReverseTransition,
      ScrollYReverseTransition,
      SlideXTransition,
      ScrollXTransition,
      SlideXReverseTransition,
      ScrollXReverseTransition,
      ScaleRotateTransition,
      ExpandXTransition,
      ExpandTransition,
    },
    setup() {
      const value = ref('Fade');
      const show = ref(true);
      function start() {
        show.value = false;
        setTimeout(() => {
          show.value = true;
        }, 300);
      }
      return { options, value, start, show };
    },
  });
</script>
<style lang="less" scoped>
  .box {
    width: 150px;
    height: 150px;
    margin-top: 20px;
    background-color: rgb(126 170 236);
  }
</style>
