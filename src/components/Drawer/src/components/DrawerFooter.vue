<template>
  <div v-if="showFooter || $slots.footer" :class="prefixCls" :style="getStyle">
    <template v-if="!$slots.footer">
      <slot name="insertFooter"></slot>
      <a-button v-if="showCancelBtn" v-bind="cancelButtonProps" class="mr-2" @click="handleClose">
        {{ cancelText }}
      </a-button>
      <slot name="centerFooter"></slot>
      <a-button
        v-if="showOkBtn"
        :type="okType"
        v-bind="okButtonProps"
        class="mr-2"
        :loading="confirmLoading"
        @click="handleOk"
      >
        {{ okText }}
      </a-button>
      <slot name="appendFooter"></slot>
    </template>

    <template v-else>
      <slot name="footer"></slot>
    </template>
  </div>
</template>
<script lang="ts" setup>
  import { computed, type CSSProperties } from 'vue';

  import { useDesign } from '@/hooks/web/useDesign';

  import { footerProps } from '../props';

  defineOptions({ name: 'BasicDrawerFooter' });

  const props = defineProps({
    ...footerProps,
    height: {
      type: String,
      default: '60px',
    },
  });

  const emit = defineEmits(['ok', 'close']);

  const { prefixCls } = useDesign('basic-drawer-footer');

  const getStyle = computed((): CSSProperties => {
    const heightStr = `${props.height}`;
    return {
      height: heightStr,
      lineHeight: `calc(${heightStr} - 1px)`,
    };
  });

  function handleOk() {
    emit('ok');
  }

  function handleClose() {
    emit('close');
  }
</script>

<style lang="less">
  @prefix-cls: ~'@{namespace}-basic-drawer-footer';
  @footer-height: 60px;
  .@{prefix-cls} {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 12px 0 20px;
    text-align: right;
    background-color: @component-background;
    border-top: 1px solid @border-color-base;

    > * {
      margin-right: 8px;
    }
  }
</style>
