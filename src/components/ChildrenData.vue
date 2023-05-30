<template>
  <div class="children-data">
    <div class="children-data__title-wrapper">
      <h1 class="children-data__title">Дети (макс. 5)</h1>

      <ActionButton v-if="children.length < 5" text="Добавить ребенка" type="outlined" @click="addChild">
        <template #icon>
          <PlusIcon primary />
        </template>
      </ActionButton>
    </div>

    <div class="children-form-wrapper">
      <ChildForm v-for="child of children" :child="child" @removeChild="removeChild" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import ChildForm from '@/components/ChildForm';
import ActionButton from '@/components/ActionButton';
import PlusIcon from '@/components/SvgIcons/PlusIcon';
import { generateRandomId } from '@/helpers/common';

export default defineComponent({
  name: 'ChildrenData',
  components: {
    ChildForm,
    ActionButton,
    PlusIcon
  },

  props: {
    children: {
      type: Array,
      default: () => {}
    }
  },

  setup(props, { emit }) {
    const addChild = () => {
      const child = {
        name: '',
        age: '',
        id: generateRandomId()
      };

      emit('addChild', child);
    };

    const removeChild = (child) => {
      emit('removeChild', child);
    };

    return { addChild, removeChild };
  }
})
</script>

<style lang="scss" scoped>
  .children-data {
    .children-form-wrapper {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    
    &__title-wrapper {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      gap: 15px;
      align-items: center;
      margin-bottom: 10px;
      min-height: 50px;
    }

    &__title {
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }
  }
</style>