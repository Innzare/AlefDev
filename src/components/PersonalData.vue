<template>
  <div class="personal-data">
    <h1 class="personal-data__title">Персональные данные</h1>

    <div class="personal-data__fields-wrapper">
      <TextField
        label='Имя'
        v-model="personalData.name"
      />
      <TextField
        label='Возраст'
        isNumberField
        v-model="personalData.age"
      />
    </div>


  </div>
</template>

<script>
import { defineComponent } from 'vue';
import TextField from '@/components/TextField';

export default defineComponent({
  name: 'PersonalData',
  components: {
    TextField
  },

  props: {
    personalData: {
      type: Object,
      default: () => {}
    }
  }
})
</script>

<style lang="scss" scoped>
  .personal-data {

    &__title {
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
      margin-bottom: 20px;
    }

    &__fields-wrapper {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
  }
</style>