<template>
  <div class="app-wrapper">
    <Header :activePage="activePage" @onChangePage="(page) => activePage = page" />

    <main class="main">
      <div class="container">
        <Form
          v-if="isFormPage"
          :children="childrenTemplate"
          :isFormChanged="isFormChanged"
          v-model:personalData="personalDataTemplate"
          @addChild="addChild"
          @removeChild="removeChild"
          @saveData="saveData"
          @cancelChanging="cancelChanging"
        />

        <Profile v-if="isProfilePage" :personalData="personalData" :children="children" />
      </div>
    </main>

    <Footer />
  </div>
</template>

<script>
import { defineComponent, ref, computed } from 'vue';
import { ACTIVE_PAGE } from '@/consts/app';
import { isObjectsEqual, getJSONParsedData } from '@/helpers/common';
import { LocalStorage } from '@/helpers/storage';
import Profile from '@/views/Profile'
import Form from '@/views/Form'
import Header from '@/components/Header'
import Footer from '@/components/Footer'

export default defineComponent({
  name: 'App',
  components: {
    Profile,
    Form,
    Header,
    Footer
  },

  setup() {
    const personalDataFromStorage = LocalStorage.get('personalData');
    const childrenFromStorage = LocalStorage.get('children');

    const isPersonalDataFromStorageExist = typeof personalDataFromStorage !== 'undefined';
    const isChildrenDataFromStorageExist = typeof childrenFromStorage !== 'undefined';

    const initialPersonalData = isPersonalDataFromStorageExist
      ? personalDataFromStorage
      : {
          name: '',
          age: ''
        }
    const initialChildrenData = isChildrenDataFromStorageExist ? childrenFromStorage : [];

    const activePage = ref(ACTIVE_PAGE.PROFILE);
    const children = ref(initialChildrenData);
    const personalData = ref(initialPersonalData);

    const childrenTemplate = ref(getJSONParsedData(children.value));
    const personalDataTemplate = ref(getJSONParsedData(personalData.value));

    const addChild = (child) => {
      childrenTemplate.value.push(child);
    }

    const removeChild = (child) => {
      childrenTemplate.value = childrenTemplate.value.filter(item => item.id !== child.id);
    }

    const saveData = () => {
      personalData.value = getJSONParsedData(personalDataTemplate.value);
      children.value = getJSONParsedData(childrenTemplate.value);

      LocalStorage.set('personalData', personalData.value);
      LocalStorage.set('children', children.value);
    }

    const cancelChanging = () => {
      personalDataTemplate.value = getJSONParsedData(personalData.value);
      childrenTemplate.value = getJSONParsedData(children.value);
    }

    const isFormChanged = computed(() => {
      return !isObjectsEqual(personalData.value, personalDataTemplate.value)
        || !isObjectsEqual(children.value, childrenTemplate.value)
    });

    const isProfilePage = computed(() => {
      return activePage.value === ACTIVE_PAGE.PROFILE
    });

    const isFormPage = computed(() => {
      return activePage.value === ACTIVE_PAGE.FORM
    });

    return {
      activePage,
      personalData,
      personalDataTemplate,
      children,
      childrenTemplate,
      isProfilePage,
      isFormPage,
      isFormChanged,
      addChild,
      removeChild,
      saveData,
      cancelChanging
    };
  }
});
</script>

<style lang='scss'>
@import "@/assets/styles/main.scss";

.app-wrapper {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.main {
  flex: 1;
  padding: 30px 0;

  @media(max-width: 768px) {
    padding-inline: 15px;
  }

  .container {
    max-width: 616px;
    margin-inline: auto;
  }
}
</style>