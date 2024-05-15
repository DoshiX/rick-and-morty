<template>
    <div class="container">
        <slot />
    </div>
</template>

<style scoped lang="scss">
.container {
    width: 100%;
    max-width: $width-conteiner + 2 * $gap-conteiner;
    margin: 0 auto;
    padding: 0 $gap-conteiner;
}
</style>