<template>
    <div id="app">

        <view-home />

    </div>
</template>

<script>
import ViewHome from "@/views/home/ViewHome";
export default {
    name: 'app',
    components: {
        ViewHome,
    },
}
</script>

<style lang="scss">
@import 'styles/global';
</style>
