<template>
    <span v-class="color" style="color:red">{{value}}</span>
</template>

<script>
    export default {
        name: "BadgeVa",
        props:{
            value : 0,
            color : "badge bg-primary"
        }
    }
</script>

<style scoped>

</style>