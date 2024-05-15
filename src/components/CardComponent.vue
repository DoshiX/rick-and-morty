<template>
    <article class="card" :id="data.id">
        <div class="card__img">
            <img class="card__img-pic" :src="data.image" :alt="data.title">
        </div>

        <div class="card__content">
            <div class="card__section">
                <h2 class="card__title">{{ data.title }}</h2>
                <span class="card__subtitle">
                    <span class="card--status-icon" :style="{ background : data.subtitle.color }"></span>
                    {{ data.subtitle.text }}
                </span>
            </div>

            <div class="card__section" v-for="item in data.topics" :key="item.title">
                <span class="card__topic">{{ item.title }}</span>
                <span class="card__subtopic">{{ item.text }}</span>
            </div>
        </div>
    </article>
</template>

<script setup>
defineProps({
    data: Object
});
</script>

<style scoped lang="scss">
.card {
    display: flex;
    flex-direction: column;

    border-radius: 10px;

    box-shadow: $card-shadow;

    color: $white;
    background-color: $card-background;

    @media #{$screen-tablet} {
        flex-direction: row;

        height: 220px;
    }

    &__img {
        position: relative;
        overflow: hidden;

        width: 100%;
        height: 300px;

        @media #{$screen-tablet} {
            width: 38%;
            height: 100%;
        }

        &-pic {
            width: 100%;
            height: 100%;

            border-radius: 10px 10px 0 0;

            object-fit: cover;
            font-family: "object-fit: cover";

            @media #{$screen-tablet} {
                border-radius: 10px 0 0 10px;
            }
        }
    }

    &__content {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        gap: 22px;
        flex-grow: 1;

        padding: 14px;
    }

    &__title {
        @include font-style(27px, 800);
        @include line-clamp-text-formated;

        @media #{$screen-tablet} {
            -webkit-line-clamp: 1;
        }
    }

    &__subtitle {
        @include font-style(16px, 500);
    }

    &__section {
        display: flex;
        flex-direction: column;
    }

    &__topic {
        @include font-style(16px, 500);

        color: $card-topic-text;
    }

    &__subtopic {
        @include font-style(18px, normal);
    }

    &--status-icon {
        display: inline-block;

        height: 9px;
        width: 9px;
        margin-right: 4px;

        border-radius: 50%;
    }
}
</style>
