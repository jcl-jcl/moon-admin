<template>
  <PageWrapper title="表单增删示例">
    <CollapseContainer title="表单增删">
      <BasicForm @register="register" @submit="handleSubmit">
        <template #add="{ field }">
          <Button v-if="Number(field) === 0" @click="add">+</Button>
          <Button v-if="Number(field) === 0" class="ml-2" @click="batchAdd">
            批量添加表单配置
          </Button>
          <Button v-if="field > 0" @click="del(field)">-</Button>
        </template>
      </BasicForm>
    </CollapseContainer>
  </PageWrapper>
</template>
<script lang="ts">
  import { Input } from 'ant-design-vue';
  import { defineComponent, ref } from 'vue';

  import { Button } from '@/components/Button';
  import { CollapseContainer } from '@/components/Container';
  import { BasicForm, useForm } from '@/components/Form/index';
  import { PageWrapper } from '@/components/Page';

  export default defineComponent({
    components: { BasicForm, CollapseContainer, PageWrapper, [Input.name]: Input, Button },
    setup() {
      const [register, { appendSchemaByField, removeSchemaByField, validate }] = useForm({
        schemas: [
          {
            field: 'field0a',
            component: 'Input',
            label: '字段0',
            required: true,
          },
          {
            field: 'field0b',
            component: 'Input',
            label: '字段0',
            required: true,
          },
          {
            field: '0',
            component: 'Input',
            label: ' ',
            slot: 'add',
          },
        ],
        labelWidth: 100,
        actionColOptions: { span: 24 },
        baseColProps: { span: 8 },
      });

      async function handleSubmit() {
        try {
          const data = await validate();
          console.log(data);
        } catch (e) {
          console.log(e);
        }
      }

      const n = ref(1);

      function add() {
        appendSchemaByField(
          {
            field: `field${n.value}a`,
            component: 'Input',
            label: '字段' + n.value,
            required: true,
          },
          '',
        );
        appendSchemaByField(
          {
            field: `field${n.value}b`,
            component: 'Input',
            label: '字段' + n.value,
            required: true,
          },
          '',
        );

        appendSchemaByField(
          {
            field: `${n.value}`,
            component: 'Input',
            label: ' ',
            slot: 'add',
          },
          '',
        );
        n.value++;
      }
      /**
       * @description: 批量添加
       */
      function batchAdd() {
        appendSchemaByField(
          [
            {
              field: `field${n.value}a`,
              component: 'Input',
              label: '字段' + n.value,
              required: true,
            },
            {
              field: `field${n.value}b`,
              component: 'Input',
              label: '字段' + n.value,
              required: true,
            },
            {
              field: `${n.value}`,
              component: 'Input',
              label: ' ',
              slot: 'add',
            },
          ],
          '',
        );
        n.value++;
      }

      function del(field) {
        removeSchemaByField([`field${field}a`, `field${field}b`, `${field}`]);
        n.value--;
      }

      return { register, handleSubmit, add, del, batchAdd };
    },
  });
</script>
