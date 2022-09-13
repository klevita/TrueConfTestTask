<template>
    <div class="wrapper">
        <LiftComponent :height='5' :currentFloor='4'/>
    </div>
</template>

<script>
import LiftComponent from './LiftComponent.vue';
export default {
    name: "HomePage",
    data: () => ({}),
    components: { LiftComponent }
}
</script>

<style scoped>
    .wrapper{
        margin:10px;
    }
</style>
