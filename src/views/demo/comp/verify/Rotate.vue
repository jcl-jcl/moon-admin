<template>
  <PageWrapper title="旋转校验示例">
    <div class="flex justify-center p-4 items-center bg-gray-700">
      <RotateDragVerify ref="el" :src="img" @success="handleSuccess" />
    </div>
  </PageWrapper>
</template>
<script lang="ts">
  import { defineComponent } from 'vue';

  import img from '@/assets/images/header.jpg';
  import { PageWrapper } from '@/components/Page';
  import { RotateDragVerify } from '@/components/Verify/index';

  export default defineComponent({
    components: { RotateDragVerify, PageWrapper },
    setup() {
      const handleSuccess = () => {
        console.log('success!');
      };
      return {
        handleSuccess,
        img,
      };
    },
  });
</script>
<style lang="less" scoped>
  .bg-gray-700 {
    background-color: #4a5568;
  }
</style>
