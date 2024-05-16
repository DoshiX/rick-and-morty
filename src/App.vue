<template>
  <div class="page">
    <Toast
      position="top-center"
      :breakpoints="toastBreakPoint"
    />

    <HeaderComponent
      class="page__header"
      v-model:selectedStatus="selectedStatus"
      v-model:characterNameInput="charactersNameInput"
      @search-click="search"
    />

    <main class="page__content">
      <Container>
        <div class="page__cards">
          <CardsGroupComponent :card-data="cardListData"/>
        </div>
      </Container>
    </main>

    <PaginatorComponent
      class="page__paginator"
      :rows="20"
      :first="currentPage"
      :total-records="count"
      @update="updatePage"
    />
  </div>
</template>

<script setup>
import CardsGroupComponent from '@/components/CardsGroupComponent.vue';
import Container from '@/components/Container.vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
import PaginatorComponent from '@/components/PaginatorComponent.vue';
import Toast from 'primevue/toast';
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useToast } from "primevue/usetoast";

const cardListData = ref([]);
const selectedStatus = ref();
const charactersNameInput = ref('');
const count = ref(0);
const currentPage = ref(0);
const searchQuery = {
  page: 1
};

const toast = useToast();
const toastBreakPoint = {
  '450px': { width: '75vw' },
};

async function updatePage(data) {
  currentPage.value = data.page;
  await search(currentPage.value + 1, true);
};

async function search(page = 1, pagination = false) {

  if (!pagination) {
    searchQuery.page = 1
    currentPage.value = 0

    if (charactersNameInput.value) {
      searchQuery.name = charactersNameInput.value;
    } else {
      searchQuery.name = ''
    };

    if (selectedStatus.value) {
      searchQuery.status = selectedStatus.value;
    } else {
      searchQuery.status = ''
    };

  } else {
    searchQuery.page = page;
  };

    await fetchListOfCharacter(searchQuery, pagination);
};

function showError(errorText) {
  toast.add({ severity: 'error', summary: 'Произошла ошибка', detail: errorText, life: 2000 });
};

async function fetchListOfCharacter(searchQuery) {
  try {
    const { data } = await axios.get(_createCharacterUrlPath(searchQuery));
    const results = data.results;
    const info = data.info;

    count.value = info.count;

    cardListData.value = [...results.map((data)=> {
      return {
        ...data,
        location: data.location.name,
        origin: data.origin.name
      }
    })];
  } catch(e) {
    const { response } = e;
    if (response.status === 404) {
      showError('Не удалось найти персонажа с такими параметрами');
      
      return
    }
    
    return showError('Неизвестная ошибка, попробуйте позже');
  } finally {
    window.scrollTo({
      top: 0,
      left: 0,
      behavior: 'smooth'
    });
  }
};

function _createCharacterUrlPath(data = { name: null, page: 1, status: null }) {
  const { name, page, status } = data;

  let url = `https://rickandmortyapi.com/api/character/?page=${page}`;

  if (name) url += `&name=${name}`;
  
  if (status) url += `&status=${status}`;
  
  return url;
}

onMounted(async () => {
    await fetchListOfCharacter();
});

</script>

<style scoped lang="scss">
.page {
  display: flex;
  flex-direction: column;

  min-height: 100vh;

  &__header {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    z-index: $z-index-header;

    flex-shrink: 0;
  }

  &__content {
    display: flex;
    flex-direction: column;
    flex-grow: 1;

    margin: $height-header-mobile 0 0;
    padding-bottom: 60px;

    background-color: $main-background;

    @media #{$screen-tablet} {
        margin-top: $height-header-tablet;
    }
  }

  &__cards {
    margin: 25px 0;
  }

  &__paginator {
    position: fixed;
    bottom: 0;
    right: 0;
    left: 0;
  }
}
</style>
