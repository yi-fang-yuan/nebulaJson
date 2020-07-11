<template>
 <div>
  <div class="user" v-for="(value,name) in jsonData" v-bind:key="value.info">
     <h1>Worker  {{name}}, host {{value.Info.Hostname}} </h1>

<!--   Each ComputerData represents a user-->
      <HardWare v-bind:computerData="value" />
  </div>
 </div>

</template>

<script>
    import HardWare from "./HardWare";
 import userInfo from '../assets/data.json'
    export default {
        name: "UserComputer",
        components: {
          HardWare
        },
        data() {
         return {
          user: userInfo,
         }
        },
        computed:{
         //return the Json data in JS object format
         jsonData : function(){
           let stringified = JSON.stringify(this.user)
           let obj = JSON.parse(stringified);
           obj = obj['result']
           return obj

         }
        }

    }
</script>

<style scoped>
 @media only screen and (max-width:768px){
  .user h1{
    padding: 50px 50px 5px 50px
  }
 }
</style>