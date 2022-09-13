<template>
    <div class="lift-wrapper" :style="{height:Height*100 + 1 + 'px'}">
        <div :ref="'lift-'+id" id="lift" class="lift">

        </div>
    </div>
</template>

<script>
export default {
    name: 'Lift',
    emits: ['ready'],
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
    watch: {
        currentFloor() {
            if (this.currentFloor !== this.lastFloor) {
                this.isDisabled = true;
                setTimeout(() => {
                    this.isDisabled = false;
                    this.$emit('ready')
                }, Math.abs(this.lastFloor - this.currentFloor)*1000);
                const el = this.$refs['lift-' + this.id]
                el.style.transition = "top " + Math.abs(this.lastFloor - this.currentFloor) + "s"  
                el.style.top = (this.Height - this.currentFloor) * 100 + 'px'
                this.lastFloor = this.currentFloor
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
        position: relative;
    }
}
</style>
