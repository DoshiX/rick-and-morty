<template>
  <div class="page">
    <header class="page__header">
      <Container>Header</Container>
    </header>

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
import { ref, onMounted } from 'vue';
import axios from 'axios';

const cardListData = ref([]);

async function fetchListOfCharacterData() {
  const { data } = await axios.get('https://rickandmortyapi.com/api/character');
  const results = data.results;

  cardListData.value.push(...results.map((data)=> {
    return {
      ...data,
      location: data.location.name,
      origin: data.origin.name
    }
  }));
};

onMounted(async () => {
    await fetchListOfCharacterData();
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

    height: 80px;

    background-color: $white;
  }

  &__content {
    display: flex;
    flex-direction: column;
    flex-grow: 1;

    margin: 80px 0 40px 0;

    background-color: $main-background;
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
