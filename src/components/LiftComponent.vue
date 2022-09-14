<template>
    <div class="lift-wrapper" :style="{height:Height*100 + 1 + 'px'}">
        <div :ref="'lift-'+id" class="lift">
            <div style="color:white;display: flex;">
                <div>{{currentFloor}}</div>
                <div style="width:10px">
                    <div v-if="currentFloor<lastFloor">v</div>
                    <div v-if="currentFloor>lastFloor">^</div>
                </div>

            </div>
            <div class="spinner" :ref="'spinner-'+id">
            </div>
        </div>
    </div>
</template>

<script>
import gsap from 'gsap'
export default {
    name: 'Lift',
    emits: ['ready', 'arrival'],
    data: () => ({
        isDisabled: false,
        lastFloor: NaN
    }),
    props: {
        id: {
            type: Number,
            required: true
        },
        Height: {
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
        this.lastFloor = this.currentFloor
        this.$refs['lift-' + this.id].style.top = (this.Height - 1) * 100 + 'px'
    },
    methods: {
        reset() {
            this.$refs['lift-' + this.id].style.transition = "none"
            this.lastFloor = this.currentFloor
            this.$refs['lift-' + this.id].style.top = (this.Height - 1) * 100 + 'px'
        }
    },
    watch: {
        currentFloor() {
            if (this.currentFloor !== this.lastFloor) {
                this.isDisabled = true;
                setTimeout(() => {
                    this.isDisabled = false;
                    this.$emit('ready')
                    this.$refs['spinner-' + this.id].style.opacity = 0
                }, (Math.abs(this.lastFloor - this.currentFloor) + 3) * 1000);
                setTimeout(() => {
                    this.$refs['spinner-' + this.id].style.opacity = 1
                    gsap.from(this.$refs['spinner-' + this.id], {
                        duration: 0.6,
                        rotation: 360,
                        repeat: 5,
                        ease: 'none'
                    })
                    this.$emit('arrival')
                    this.lastFloor = this.currentFloor
                }, Math.abs(this.lastFloor - this.currentFloor) * 1000);
                const el = this.$refs['lift-' + this.id]
                el.style.transition = "top " + Math.abs(this.lastFloor - this.currentFloor) + "s"
                el.style.top = (this.Height - this.currentFloor) * 100 + 'px'
            }

        },
        Height() {
            const el = this.$refs['lift-' + this.id]
            el.style.transition = "top 0s"
            el.style.top = (this.Height - 1) * 100 + 'px'
        }
    }

}
</script>

<style scoped lang="scss">
.lift-wrapper {
    display: flex;
    background-color: darkgrey;

    .lift {
        height: 100px;
        width: 50px;
        background-color: black;
        display: flex;
        justify-content: space-around;
        align-items: center;
        position: relative;
        flex-direction: column;

        .spinner {
            opacity: 0;
            width: 20px;
            height: 20px;
            background-color: black;
            border-radius: 20px;
            border-right: 2px solid white;
        }
    }
}
</style>
