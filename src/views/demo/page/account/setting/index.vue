<template>
  <ScrollContainer>
    <div ref="wrapperRef" :class="prefixCls">
      <Tabs tab-position="left" :tabBarStyle="tabBarStyle">
        <template v-for="item in settingList" :key="item.key">
          <TabPane :tab="item.name">
            <component :is="item.component" />
          </TabPane>
        </template>
      </Tabs>
    </div>
  </ScrollContainer>
</template>

<script lang="ts">
  import { Tabs } from 'ant-design-vue';
  import { defineComponent } from 'vue';

  import { ScrollContainer } from '@/components/Container/index';

  import AccountBind from './AccountBind.vue';
  import BaseSetting from './BaseSetting.vue';
  import { settingList } from './data';
  import MsgNotify from './MsgNotify.vue';
  import SecureSetting from './SecureSetting.vue';

  export default defineComponent({
    components: {
      ScrollContainer,
      Tabs,
      TabPane: Tabs.TabPane,
      BaseSetting,
      SecureSetting,
      AccountBind,
      MsgNotify,
    },
    setup() {
      return {
        prefixCls: 'account-setting',
        settingList,
        tabBarStyle: {
          width: '220px',
        },
      };
    },
  });
</script>
<style lang="less">
  .account-setting {
    margin: 12px;
    background-color: @component-background;

    .base-title {
      padding-left: 0;
    }

    .ant-tabs-tab-active {
      background-color: @primary-color;
    }
  }
</style>
