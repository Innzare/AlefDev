<template>
  <header class="header">
    <img class="logo" src="@/assets/img/alefLogo.svg" alt="Logo">

    <button
      v-for="page in pages"
      :key="page.text"
      @click="$emit('onChangePage', page.value)"
      :class="{
        'button': true,
        'button--active': page.isActive
      }"
    >
        {{ page.text }}
    </button>
  </header>
</template>

<script>
import { defineComponent, ref, computed, watch } from 'vue';
import { ACTIVE_PAGE } from '@/consts/app';

export default defineComponent({
  name: 'Header',
  props: {
    activePage: {
      type: String,
      default: ''
    }
  },

  setup(props) {
    const { activePage } = props;

    const getPages = (value) => {
      return [
        {
          text: "Форма",
          value: ACTIVE_PAGE.FORM,
          isActive: value === ACTIVE_PAGE.FORM
        },
        {
          text: "Превью",
          value: ACTIVE_PAGE.PROFILE,
          isActive: value === ACTIVE_PAGE.PROFILE
        }
      ];
    }

    const pages = ref(getPages(activePage));

    watch(() => props.activePage, (newValue) => {
      pages.value = getPages(newValue);
    });

    return {
      pages
    }
  }
})
</script>

<style lang="scss" scoped>
  .header {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid rgba(17, 17, 17, 0.1);
    width: 100%;
    gap: 12px;
    height: 76px;

    .logo {
      position: absolute;
      left: 50px;

      @media(max-width: 768px) {
        display: none;
      }
    }

    .button {
      width: 85px;
      font-style: normal;
      font-weight: 400;
      font-size: 14px;
      line-height: 24px;
      color: rgba(17, 17, 17, 0.48);
      padding: 4px 12px;
      border: 1px solid transparent;
      border-radius: 4px;
      transition: all .2s ease-in-out;

      &:hover {
        color: #01A7FD;
        border-color: #01A7FD;
      }

      &--active {
        color: #01A7FD;
        border-color: #01A7FD;
      }
    }
  }
</style>