<template>
    <div>
        <div>
            Кол-во этажей: <input style="width:100px" v-model="Height">
        </div>
        <div class="house">
            <div class="floors">
                <div class="floor" v-for="h in Height" :id="'floor-'+h">
                    {{ -(-Height + h)+1 }}
                </div>
                <div style="height:1px;background-color:black"> </div>
            </div>
            <div class="lifts">
                <LiftComponent ref="lft" :Height='Height' :id="0" :currentFloor='liftCalls[0]' />
            </div>
            <div class="buttons">
                <button class="button" @click="callLift(-(-Height + h)+1)" v-for="h in Height" :id="'button-'+h">

                </button>
            </div>
        </div>
    </div>
</template>

<script>
import LiftComponent from './LiftComponent.vue';
export default {
    name: "HomePage",
    data: () => ({
        Height: 5,
        Lifts:3,
        liftCalls: [],
        liftQueue:[]
    }),
    components: { LiftComponent },
    methods: {
        callLift(floor) {
            if(!this.$refs.lft.isDisabled){
                this.liftCalls[0] = floor
            }
        }
    },
    watch: {
        Height() {
            if (this.Height === '' || isNaN(this.Height)){
                this.Height = 1
            } else {
                this.Height = parseInt(this.Height) >= 1 ? parseInt(this.Height) : -parseInt(this.Height);
            }
        }
    }
}
</script>

<style scoped lang="scss">
.house {
    margin: 10px;
    display: flex;
    .lifts{
        display: flex;
        & *{
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
            width: 20px;

            height: 20px;
            background-color: green;
            border: none;
            padding: 0;
        }
    }
}
</style>
