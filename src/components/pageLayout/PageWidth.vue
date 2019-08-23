<template>
    <component :is="tag"
               class="page-width"
    >
        <slot />
    </component>
</template>

<script>
export default {
    name: 'PageWidth',
    props: {
        tag: {
            type: String,
            default: 'div',
        },
    }
}
</script>

<style scoped lang="scss">
.page-width {
    max-width: $page-width-max;
    margin-right: auto;
    margin-left: auto;
    padding-right: $page-padding-sides-xl;
    padding-left: $page-padding-sides-xl;
    transition: all .1s ease-in-out;
    transition-property: padding-left, padding-right;

    @include respond-to($media-xl, max) {
        padding-right: $page-padding-sides-lg;
        padding-left: $page-padding-sides-lg;
    }

    @include respond-to($media-md, max) {
        padding-right: $page-padding-sides-md;
        padding-left: $page-padding-sides-md;
    }

    @include respond-to($media-sm, max) {
        padding-right: $page-padding-sides-sm;
        padding-left: $page-padding-sides-sm;
    }
}
</style>