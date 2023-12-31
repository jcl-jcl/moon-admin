<template>
  <Teleport to="body">
    <transition name="zoom-fade" mode="out-in">
      <div v-if="open" :class="getClass" @click.stop>
        <div v-click-outside="handleClose" :class="`${prefixCls}-content`">
          <div :class="`${prefixCls}-input__wrapper`">
            <a-input
              ref="inputRef"
              :class="`${prefixCls}-input`"
              :placeholder="t('common.searchText')"
              allow-clear
              @change="handleSearch"
            >
              <template #prefix>
                <SearchOutlined />
              </template>
            </a-input>
            <span :class="`${prefixCls}-cancel`" @click="handleClose">
              {{ t('common.cancelText') }}
            </span>
          </div>

          <div v-show="getIsNotData" :class="`${prefixCls}-not-data`">
            {{ t('component.app.searchNotData') }}
          </div>

          <ul v-show="!getIsNotData" ref="scrollWrap" :class="`${prefixCls}-list`">
            <li
              v-for="(item, index) in searchResult"
              :ref="setRefs(index)"
              :key="item.path"
              :data-index="index"
              :class="[
                `${prefixCls}-list__item`,
                {
                  [`${prefixCls}-list__item--active`]: activeIndex === index,
                },
              ]"
              @mouseenter="handleMouseenter"
              @click="handleEnter"
            >
              <div :class="`${prefixCls}-list__item-icon`">
                <Icon :icon="item.icon || 'mdi:form-select'" :size="20" />
              </div>
              <div :class="`${prefixCls}-list__item-text`">
                {{ item.name }}
              </div>
              <div :class="`${prefixCls}-list__item-enter`">
                <Icon icon="ant-design:enter-outlined" :size="20" />
              </div>
            </li>
          </ul>
          <AppSearchFooter />
        </div>
      </div>
    </transition>
  </Teleport>
</template>

<script lang="ts" setup>
  import { SearchOutlined } from '@ant-design/icons-vue';
  import { useRefs } from '@vben/hooks';
  import { computed, nextTick, ref, unref, watch } from 'vue';

  import Icon from '@/components/Icon/Icon.vue';
  import vClickOutside from '@/directives/clickOutside';
  import { useAppInject } from '@/hooks/web/useAppInject';
  import { useDesign } from '@/hooks/web/useDesign';
  import { useI18n } from '@/hooks/web/useI18n';

  import AppSearchFooter from './AppSearchFooter.vue';
  import { useMenuSearch } from './useMenuSearch';

  const props = defineProps({
    open: { type: Boolean },
  });

  const emit = defineEmits(['close']);

  const scrollWrap = ref(null);
  const inputRef = ref<HTMLElement | null>(null);

  const { t } = useI18n();
  const { prefixCls } = useDesign('app-search-modal');
  const { refs, setRefs } = useRefs();
  const { getIsMobile } = useAppInject();

  const { handleSearch, searchResult, keyword, activeIndex, handleEnter, handleMouseenter } =
    useMenuSearch(refs, scrollWrap, emit);

  const getIsNotData = computed(() => !keyword || unref(searchResult).length === 0);

  const getClass = computed(() => {
    return [
      prefixCls,
      {
        [`${prefixCls}--mobile`]: unref(getIsMobile),
      },
    ];
  });

  watch(
    () => props.open,
    (open: boolean) => {
      open &&
        nextTick(() => {
          unref(inputRef)?.focus();
        });
    },
  );

  function handleClose() {
    searchResult.value = [];
    emit('close');
  }
</script>
<style lang="less" scoped>
  @prefix-cls: ~'@{namespace}-app-search-modal';
  @footer-prefix-cls: ~'@{namespace}-app-search-footer';
  .@{prefix-cls} {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 800;
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    padding-top: 50px;
    background-color: rgb(0 0 0 / 25%);

    &--mobile {
      padding: 0;

      > div {
        width: 100%;
      }

      .@{prefix-cls}-input {
        width: calc(100% - 38px);
      }

      .@{prefix-cls}-cancel {
        display: inline-block;
      }

      .@{prefix-cls}-content {
        width: 100%;
        height: 100%;
        border-radius: 0;
      }

      .@{footer-prefix-cls} {
        display: none;
      }

      .@{prefix-cls}-list {
        height: calc(100% - 80px);
        max-height: unset;

        &__item {
          &-enter {
            opacity: 0 !important;
          }
        }
      }
    }

    &-content {
      position: relative;
      flex-direction: column;
      width: 632px;
      margin: 0 auto auto;
      background-color: @component-background;
      border-radius: 16px;
      box-shadow: 0 25px 50px -12px rgb(0 0 0 / 25%);
    }

    &-input__wrapper {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 14px 14px 0;
    }

    &-input {
      width: 100%;
      height: 48px;
      font-size: 1.5em;
      color: #1c1e21;
      border-radius: 6px;

      span[role='img'] {
        color: #999;
      }
    }

    &-cancel {
      display: none;
      font-size: 1em;
      color: #666;
    }

    &-not-data {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100px;
      font-size: 0.9;
      color: rgb(150 159 175);
    }

    &-list {
      max-height: 472px;
      padding: 0 14px;
      padding-bottom: 20px;
      margin: 0 auto;
      margin-top: 14px;
      overflow: auto;

      &__item {
        position: relative;
        display: flex;
        align-items: center;
        width: 100%;
        height: 56px;
        padding-bottom: 4px;
        padding-left: 14px;
        margin-top: 8px;
        font-size: 14px;
        color: @text-color-base;
        cursor: pointer;
        background-color: @component-background;
        border-radius: 4px;
        box-shadow: 0 1px 3px 0 #d4d9e1;

        > div:first-child,
        > div:last-child {
          display: flex;
          align-items: center;
        }

        &--active {
          color: #fff;
          background-color: @primary-color;

          .@{prefix-cls}-list__item-enter {
            opacity: 1;
          }
        }

        &-icon {
          width: 30px;
        }

        &-text {
          flex: 1;
        }

        &-enter {
          width: 30px;
          opacity: 0;
        }
      }
    }
  }
</style>
