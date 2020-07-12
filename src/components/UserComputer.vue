<template>
 <div>
  <div class="user" v-for="(value,name) in this.user" v-bind:key="value.info">
     <h1>Worker  {{name}}, host {{value.Info.Hostname}} </h1>

<!--   Each ComputerData represents a user-->
      <HardWare v-bind:computerData="value" />
  </div>
 </div>

</template>

<script>
    import axios from "axios";
    import HardWare from "./HardWare";
    export default {
        name: "UserComputer",
        components: {
          HardWare
        },
        data() {
         return {
          user: {},
         }
        },

        //  Fetch Data from Json Application
        async created(){
            const config = {
              headers:{
                Accept: "application/json"
              }
            };
            try {
              const res = await axios.post("https://nbtest.free.beeceptor.com/miner",config);
              console.log(res.data.result)
              this.user = res.data.result
            
            }catch(err){
              console.log(err);
            }

        },

    }
</script>

<style scoped>
 @media only screen and (max-width:768px){
  .user h1{
    padding: 50px 50px 5px 50px
  }
 }
</style>