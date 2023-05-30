<template>
  <div class="child-form">
    <div class="child-form__fields-wrapper">
      <TextField label='Имя' v-model="child.name" />
      <TextField label='Возраст' v-model="child.age" isNumberField />

      <button @click="removeChild">Удалить</button>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import TextField from '@/components/TextField';

export default defineComponent({
  name: 'ChildForm',
  components: {
    TextField
  },

  props: {
    child: {
      type: Object,
      default: () => {}
    }
  },

  setup(props, { emit }) {
    const removeChild = () => {
      emit('removeChild', props.child);
    };

    return { removeChild };
  }
})
</script>

<style lang="scss" scoped>
  .child-form {
    animation-name: fade-in;
    animation-duration: 0.5s;
    animation-fill-mode: forwards;

    &__title {
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
      margin-bottom: 20px;
    }

    &__fields-wrapper {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 18px;

      @media(max-width: 768px) {
        gap: 8px;
      }

      button {
      font-weight: 400;
      font-size: 14px;
      line-height: 24px;
      color: #01A7FD;
      border-bottom: 1px solid transparent;
      transition: all .2s ease-in-out;

      &:hover {
        border-color: #01A7FD;
      }
    }
    }
  }
</style>