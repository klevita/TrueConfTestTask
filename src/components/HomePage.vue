<template>
    <div class="wrapper">
        <LiftGroup/>
    </div>
</template>

<script>
import LiftGroup from './LiftGroupComponent.vue';
export default {
    name: "HomePage",
    data: () => ({}),
    components: { LiftGroup }
}
</script>

<style scoped>
    .wrapper{
        margin:10px;
    }
</style>
