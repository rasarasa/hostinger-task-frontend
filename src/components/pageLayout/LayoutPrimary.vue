<template>
    <div class="layout">

        <page-header theme="transparent" />

        <slot />

    </div>
</template>

<script>
import PageHeader from "@/components/pageLayout/PageHeader";

export default {
    name: 'LayoutPrimary',
    components: {
        PageHeader,
    },
}
</script>

<style scoped lang="scss">
.page {
    //
}
</style>