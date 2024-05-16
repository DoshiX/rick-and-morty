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

    <footer class="page__footer">
      <Container>Footer</Container>
    </footer>
  </div>
</template>

<script setup>
import CardsGroupComponent from '@/components/CardsGroupComponent.vue';
import Container from '@/components/Container.vue';
import HeaderComponent from '@/components/HeaderComponent.vue'
import Toast from 'primevue/toast';
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useToast } from "primevue/usetoast";

const cardListData = ref([]);
const selectedStatus = ref();
const charactersNameInput = ref('');

const toast = useToast();
const toastBreakPoint = {
  '450px': { width: '75vw' },
};

async function search() {
    const searchQuery = {
      page: 1
    };

    if (charactersNameInput.value) {
      searchQuery.name = charactersNameInput.value;
    }

    if (selectedStatus.value) {
      searchQuery.status = selectedStatus.value;
    }

    await fetchListOfCharacter(searchQuery);
};

function showError(errorText) {
  toast.add({ severity: 'error', summary: 'Произошла ошибка', detail: errorText, life: 2000 });
};

async function fetchListOfCharacter(searchQuery) {
  try {
    const { data } = await axios.get(_createCharacterUrlPath(searchQuery));
    const results = data.results;
    // const info = data.info;

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

    margin: $height-header-mobile 0 $gap-conteiner 0;

    background-color: $main-background;

    @media #{$screen-tablet} {
        margin-top: $height-header-tablet;
    }
  }

  &__cards {
    margin: 25px 0;
  }

  &__footer {
    position: fixed;
    bottom: 0;
    right: 0;
    left: 0;

    height: 40px;

    background-color: $white;
  }
}
</style>
