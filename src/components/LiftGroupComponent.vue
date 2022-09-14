<template>
    <div>
        <div>
            Кол-во этажей: <input style="width:100px;border:1px solid grey" v-model="Height">
        </div>
        <div style="height:10px"></div>
        <div>
            Кол-во лифтов: <input style="width:96px;border:1px solid grey" v-model="Lifts">
        </div>
        <div class="house">
            <div class="floors">
                <div class="floor" v-for="h in Height" :id="'floor-'+h">
                    {{ -(-Height + h)+1 }}
                </div>
                <div style="height:1px;background-color:black"> </div>
            </div>
            <div class="lifts">
                <LiftComponent v-for="n in Lifts" @arrival="checkButtons()" @ready="invokeLifts()" ref="lft"
                    :Height='Height' :id="n-1" :currentFloor='liftCalls[n-1]' />
            </div>
            <div class="buttons">
                <button class="button" :style="buttonColors[-(-Height + h)+1 ]" @click="callHandler(-(-Height + h)+1);"
                    v-for="h in Height"></button>
            </div>
        </div>
    </div>
</template>

<script>
import LiftComponent from './LiftComponent.vue';
export default {
    name: "LiftGroup",
    data: () => ({
        Height: 5,
        lastLifts: 2,
        Lifts: 2,
        liftCalls: [1, 1],
        liftQueue: [],
        buttonColors: []
    }),
    components: { LiftComponent },
    methods: {
        checkButtons() {
            this.buttonColors = []
            // for (let n = 0; n < this.Lifts; n++) {
            //     this.buttonColors[this.$refs.lft[n].currentFloor] = { backgroundColor: 'red' }
            // }
            this.liftCalls.forEach((l) => {
                this.buttonColors[l] = { backgroundColor: 'red' }
            })
            this.liftQueue.forEach((l) => {
                this.buttonColors[l] = { backgroundColor: 'red' }
            })

        },
        invokeLifts() {
            let minRange = 99999
            if (this.liftQueue.length) {
                for (let n = 0; n < this.Lifts; n++) {
                    if (!this.$refs.lft[n].isDisabled && Math.abs(this.$refs.lft[n].currentFloor - this.liftQueue[this.liftQueue.length - 1]) < minRange) {
                        minRange = Math.abs(this.$refs.lft[n].currentFloor - this.liftQueue[this.liftQueue.length - 1])
                    }
                }
                for (let n = 0; n < this.Lifts; n++) {
                    if (!this.$refs.lft[n].isDisabled && Math.abs(this.$refs.lft[n].currentFloor - this.liftQueue[this.liftQueue.length - 1]) === minRange) {
                        this.liftCalls[n] = this.liftQueue[this.liftQueue.length - 1]
                        this.liftQueue.pop()
                        break
                    }
                }

            }
            this.checkButtons()
        },
        callHandler(floor) {
            for (let n = 0; n < this.Lifts; n++) {
                if (this.$refs.lft[n].currentFloor === floor) {
                    return
                }
            }
            if (!~this.liftQueue.indexOf(floor)) {
                this.liftQueue.push(floor)
            }
            this.invokeLifts()
        }

    },
    mounted() {
        this.checkButtons()
    },
    watch: {
        Height() {
            if (this.Height === '' || isNaN(this.Height)) {
                this.Height = 1
            } else {
                this.Height = parseInt(this.Height) >= 1 ? parseInt(this.Height) : -parseInt(this.Height);
            }
            for (let n = 0; n < this.Lifts; n++) {
                this.liftCalls[n] = 1
            }
            this.liftQueue = []
            this.checkButtons()
        },
        Lifts() {
            if (this.Lifts === '' || isNaN(this.Lifts)) {
                this.Lifts = 1
            } else {
                this.Lifts = parseInt(this.Lifts) >= 1 ? parseInt(this.Lifts) : -parseInt(this.Lifts);
            }
            for (let n = 0; n < this.lastLifts; n+=1) {
                this.liftCalls[n] = 1
                this.$refs.lft[n].reset()
            }
            this.liftQueue = []
            this.checkButtons()
            this.lastLifts = this.lifts
        },
    }
}
</script>

<style scoped lang="scss">
.house {
    margin: 10px;
    display: flex;

    .lifts {
        display: flex;

        & * {
            margin-left: 5px;
        }
    }

    .floors {
        flex-direction: column;
        display: flex;

        .floor {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 99px;
            width: 20px;
            border-top: 1px solid black;
            border-left: 1px solid black;
        }
    }

    .buttons {
        flex-direction: column;
        display: flex;
        justify-content: space-around;
        align-items: center;
        width: 40px;

        .button {
            border-radius: 2px;
            width: 20px;
            position: relative;
            box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.8);
            height: 20px;
            background-color: green;
            border: none;
            padding: 0;

            &:active {
                box-shadow: none;
                right: -2px;
                top: 2px;
            }
        }
    }
}
</style>
