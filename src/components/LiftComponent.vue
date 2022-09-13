<template>
    <div class="house" :style="{height:height*100 + 'px'}">
        <div class="floors">
            <div class="floor" v-for="h in height">
                {{ -(-height + h)+1 }}
            </div>
            <div style="height:1px;background-color:black"> </div>
        </div>
        <div :ref="'lift-'+id" id="lift" class="lift">

        </div>
    </div>
</template>

<script>
import gsap from 'gsap'
export default {
    name: 'HomePage',
    data: () => ({
        lastFloor: 1
    }),
    props: {
        id: {
            type: Number,
            required: true
        },
        height: {
            type: Number,
            required: true
        },
        currentFloor: {
            type: Number,
            required: false,
            default: 1
        }
    },
    mounted() {
        this.lastFloor = 2
        // setTimeout(() => {
        //     gsap.to(this.$refs['lift-' + this.id], { y: -(this.currentFloor - this.lastFloor) * 100, duration: 1, ease: 'inOut' })
        // }, 1500)
    },
    watch: {
        currentFloor() {
            gsap.to(this.$refs['lift-' + this.id], { y: 400, duration: 1, ease: 'inOut' })
        }
    }

}
</script>

<style scoped lang="scss">
.house {
    display: flex;

    .floors {
        flex-direction: column;
        display: flex;
        .floor {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100px;
            width: 20px;
            border-top:1px solid black;
            border-left:1px solid black;
        }
    }

    .lift {
        height: 100px;
        width: 50px;
        background-color: darkgrey;
        position: relative;
    }
}
</style>
