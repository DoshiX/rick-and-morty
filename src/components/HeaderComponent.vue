<template>
    <header class="header">
      <Container class="header__container">
        <div class="header__search">
            <InputText
                placeholder="Кого ищем?"
                v-model="characterNameInput"
            />

            <Dropdown
                placeholder="Статус"
                v-model="selectedStatus"
                :options="STATUS"
                showClear
            />

            <Button
                label="Применить"
                icon="pi pi-check"
                iconPos="right"
                @click="$emit('searchClick')"
            />
        </div>
      </Container>
    </header>
</template>

<script setup>
import Container from '@/components/Container.vue';
import { STATUS } from '@/constants';

defineEmits(['searchClick']);

const selectedStatus = defineModel('selectedStatus');
const characterNameInput = defineModel('characterNameInput');

</script>

<style scoped lang="scss">
.header {
    height: $height-header-mobile;

    background-color: $header-background;
    backdrop-filter: blur(10px);

    @media #{$screen-tablet} {
        height: $height-header-tablet;
    }

    &__container {
        height: 100%;
    }

    &__search {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 15px;

        margin-top: 10px;

        @media #{$screen-tablet} {
            flex-direction: row;

            height: 100%;
            margin: 0;
        }
    }

    &:deep(.p-inputtext) {
        width: 100%;
        flex-basis: 35%;
    }

    &:deep(.p-dropdown) {
        width: 100%;
        flex-basis: 35%;
    }
}
</style>
