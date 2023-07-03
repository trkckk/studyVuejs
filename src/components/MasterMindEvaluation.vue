<template>
    <Bootstrap-Badge color="bg-danger" v-if="move.partial > 0" :value="move.partial"/>
    <Bootstrap-Badge color="bg-succes" v-if="move.perfect > 0" :value="move.perfect"/>
    <Bootstrap-Badge color="bg-warning" v-if="move.perfect === 0 && move.partial === 0" :value="NO MATCH"/>
</template>

<script>
    export default {
        component: {
            BootstrapBadge
        },
        name: "MasterMindEvaluation",
        props:["move"]
    }

</script>

<style scoped>

</style>