<template>
    <div class="container">
        <div class="column">
        <h4>CPU</h4>
        <Progress
                strokeColor="#FF00AA"
                :transitionDuration="3000"
                :radius="60"
                :strokeWidth="10"
                :value="`${getCPU[0]}`"
        >
            <template v-slot:footer>
                <b>{{getCPU[1]}} Core(s) in Used</b>
            </template>
        </Progress>
        </div>
        <div class="column">
            <h4>RAM</h4>
            <Progress
                    strokeColor="#00FFFF"
                    :transitionDuration="3000"
                    :radius="60"
                    :strokeWidth="10"
                    :value="`${getRAM[0]}`"
            >
                <template v-slot:footer>
                    <b>{{getRAM[2]}} GiB/{{getRAM[1]}} GiB</b>
                </template>
            </Progress>
        </div>
        <div class="column">
            <h4>VMEM</h4>
            <Progress :transitionDuration="3000"
                      :radius="60"
                      :strokeWidth="10"
                      :value="`${getVirtual[0]}`">
                <template v-slot:footer>
                    <b>{{getVirtual[1]}} GiB/{{getVirtual[2]}} GiB</b>
                </template>
            </Progress>
        </div>
        <div class="column" v-if="this.computerData.Info.Resources.GPUs.length > 0">
            <h4>GPU</h4>
            <Progress :transitionDuration="3000"
                      strokeColor="#DEB887"
                      :radius="60"
                      :strokeWidth="10"
                      value="100"
            >
                <div class="content">{{this.computerData.Info.Resources.GPUs[0]}}</div>
                <template v-slot:footer class="">
                    <b>GPU of User</b>
                </template>
            </Progress>
        </div>
    </div>
</template>

<script>
    import Progress from "../index.vue";
    export default {
        name: "HardWare",
        components:{
            Progress,
        },
        props:["computerData"],
        computed:{
            //Each of the following method calculates the usage of each category including their representation in percentage
            //I made sure to leave only 2 decimal places so it is more user friendly.
            getRAM:function(){
                let ramTotal, ramUsed, ramPercent
                ramTotal = this.computerData.Info.Resources.MemPhysical*(9.3132*10**(-10))
                ramUsed = (this.computerData.Info.Resources.MemReserved + this.computerData.MemUsedMin) *(9.3132*10**(-10))
                ramPercent = (ramUsed/ramTotal) * 100
                ramPercent = ramPercent.toFixed()
                ramTotal = ramTotal.toFixed(2)
                ramUsed = ramUsed.toFixed(2)
                return [ramPercent,ramTotal,ramUsed]
            },
            getCPU (){
                let cpuPercent, cpuUsed
                cpuUsed = this.computerData.CpuUse
                cpuPercent = (this.computerData.CpuUse/ this.computerData.Info.Resources.CPUs) * 100
                return [cpuPercent, cpuUsed]
            },
            getVirtual: function(){
                let virtualPercent, virtualUsed, virtualTotal
                virtualUsed = (this.computerData.Info.Resources.MemReserved + this.computerData.MemUsedMax) * (9.3132*10**(-10))
                virtualTotal =  (this.computerData.Info.Resources.MemPhysical +  this.computerData.Info.Resources.MemSwap) * (9.3132*10**(-10))
                virtualPercent = (virtualUsed/virtualTotal) * 100
                virtualPercent = virtualPercent.toFixed()
                virtualUsed = virtualUsed.toFixed()
                virtualTotal = virtualTotal.toFixed()
                return [virtualPercent, virtualUsed, virtualTotal]
            },
        }

    }
</script>

<style scoped>
    .container{
       display:flex;
        flex-direction: row;
       justify-content: space-between;
        margin: 0 10% 0 10%
    }
    .column{
        margin-bottom: 7%;
    }

    @media only screen and (max-width:768px){
        .container{
            display:flex;
            flex-direction: column;
        }
        .column{
            margin-bottom: 10%;
        }
    }


</style>