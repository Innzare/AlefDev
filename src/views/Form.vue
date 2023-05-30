<template>
  <div class="form">
    <PersonalData v-model:personalData="personalData" />
    <ChildrenData v-model:children="children" @addChild="addChild" @removeChild="removeChild" />

    <div class="save-button-wrapper">
      <ActionButton text="Сохранить" type="filled" @click="$emit('saveData')" :disabled="!isFormChanged" />
      <ActionButton v-if="isFormChanged" text="Отменить" @click="$emit('cancelChanging')" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import PersonalData from '@/components/PersonalData';
import ChildrenData from '@/components/ChildrenData';
import ActionButton from '@/components/ActionButton';

export default defineComponent({
  name: 'Form',
  components: {
    PersonalData,
    ChildrenData,
    ActionButton
  },

  props: {
    personalData: {
      type: Object,
      default: () => {}
    },
    children: {
      type: Array,
      default: []
    },
    isFormChanged: Boolean
  },

  setup(props, { emit }) {
    const addChild = (child) => {
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
  .form {
    display: flex;
    flex-direction: column;
    gap: 33px;
    animation-name: fade-in;
    animation-duration: 0.5s;
    animation-fill-mode: forwards;

    .save-button-wrapper {
      display: flex;
      justify-content: flex-start;
      gap: 10px;
    }
  }
</style>