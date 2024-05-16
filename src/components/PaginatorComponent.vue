<template>
    <div class="paginator">
        <Container>
            <Paginator
                :rows="rows"
                :totalRecords="totalRecords"
                :first="first * 20"
                :pageLinkSize="pageLinkSize"
                @page="onUpdatePage"
            />
        </Container>
    </div>
</template>

<script setup>
import Container from '@/components/Container.vue';
import { onMounted, ref } from 'vue';

defineProps({
    rows: Number,
    totalRecords: Number,
    first: Number
});

const emit = defineEmits(['update']);
const pageLinkSize = ref(5);

function onUpdatePage(data) {
    emit('update', data);
};

onMounted(() => {
    window.addEventListener('resize', (e) => {
        const width= document.body.clientWidth;
        if (width <= 395) {
            pageLinkSize.value = 3
        } else {
            pageLinkSize.value = 5
        };
    })
});


</script>

<style scoped lang="scss">
.paginator {
    &:deep(.p-paginator) {
        margin: 0 -40px;

        @media #{$screen-tablet} {
            margin: 0 -15px;
        }

        @media #{$screen-desktop} {
            margin: 0;
        }
    }
}
</style>
