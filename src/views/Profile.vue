<template>
  <div class="profile">
    <div class="profile__personal-data-wrapper">
      <h1 class="profile__title">Персональные данные</h1>

      <p class="profile__personal-data">{{ userInfo }}</p>
    </div>

    <div class="profile__children">
      <h1 class="profile__title">Дети</h1>

      <template v-if="isChildrenExist">
        <div class="profile__child" v-for="child of children">{{ getChildrenInfo(child) }}</div>
      </template>

      <div v-else class="profile__no-children">Дети не указаны</div>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed } from 'vue';
import { getAgeFormatted } from '@/helpers/common';

export default defineComponent({
  name: 'Profile',

  props: {
    personalData: {
      type: Object,
      default: () => {}
    },

    children: {
      type: Array,
      default: []
    }
  },

  setup(props) {
    const { personalData, children } = props;

    const userInfo = computed(() => {
      const { name, age } = personalData;

      const isInfoExist = name !== '' || age !== '';

      const ageFormatted = getAgeFormatted(age)
      const userName = name !== '' ? name : 'Имя не указано';
      const userAge = age !== '' ? ageFormatted : 'Возраст не указан';
      const userInfo = `${userName}, ${userAge}`;

      return isInfoExist
        ? userInfo
        : 'Введите данные в форме';
    });

    const isChildrenExist = computed(() => children.length > 0);

    const getChildrenInfo = (child) => {
      const { name, age } = child;

      const isInfoExist = name !== '' || age !== '';

      const ageFormatted = getAgeFormatted(age)
      const childName = name !== '' ? name : 'Имя не указано';
      const childAge = age !== '' ? ageFormatted : 'Возраст не указан';
      const childInfo = `${childName}, ${childAge}`;

      return isInfoExist
        ? childInfo
        : 'Введите данные в форме';
    };

    return { userInfo, children, isChildrenExist, getChildrenInfo }
  }
})
</script>

<style lang="scss" scoped>
  .profile {
    animation-name: fade-in;
    animation-duration: 0.5s;
    animation-fill-mode: forwards;

    &__title {
      font-weight: 500;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__personal-data-wrapper {
      display: flex;
      flex-direction: column;
      gap: 20px;
      margin-bottom: 60px;
    }

    &__personal-data {
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__children {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 20px;
    }

    &__no-children {
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }

    &__child {
      padding: 10px 20px;
      background: #F1F1F1;
      border-radius: 5px;
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 24px;
      color: #111111;
    }
  }
</style>