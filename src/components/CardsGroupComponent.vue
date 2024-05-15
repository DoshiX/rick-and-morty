<template>
    <div class="cards__list">
        <div class="cards__item" v-for="item in cardData" :key="item.id">
            <CardComponent  
                :data="formatData(item)"
            />
        </div>
    </div>
</template>

<script setup>
import CardComponent from './CardComponent.vue';
import { STATUS_COLOR } from '@/constants';

defineProps({
    cardData: Array
});

const formatData = (item) => {
    return {
        image: item.image,
        id: item.id,
        title: item.name,
        subtitle: {
            color: STATUS_COLOR[item.status],
            text: `${ item.status } - ${ item.species }`
        },
        topics: [
            {
                title: 'Last known location:',
                text: item.origin,
            },
            {
                title: 'First seen in:',
                text: item.location,
            }
        ]
    };
};
</script>

<style scoped lang="scss">
.cards {
    &__list {
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;

        margin-top: -25px;

        @media #{$screen-tablet} {
            flex-direction: row;

            margin-left: -15px;
            margin-right: -15px;
        }
    }

    &__item {
        width: 100%;
        margin-top: 25px;

        @media #{$screen-desktop} {
            width: math.percentage(math.div(1,2));
            
            padding-right: 15px;
            padding-left: 15px;
        }
    }
}
</style>
