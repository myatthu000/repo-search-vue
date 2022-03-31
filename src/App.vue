<!-- vue3 composition api practice  -->
<!-- github repositery search project  -->

<template>
  <div class="flex flex-col items-center w-1/2 mx-auto">
    <!-- <img alt="Vue logo" class="w-10" src="./assets/logo.png"> -->
    <h3 class="font-medium text-xl">Enter Github User Name</h3>
    <input placeholder="search repo ..." @keyup.enter="newName = name" v-model="name" type="text" name="" class="ml-4 p-1 border-solid shadow shadow-outline mt-4" id="">
    <button 
    @click="newName = name"
    class="border border-red-700 mt-4 w-32 rounded-md p-2 mb-10 uppercase font-serif">
      Press me
    </button>

    <p 
    v-if="data.length >= 1">
        Repo of : {{newName}} | {{data.length}}
    </p>
    <!-- <p v-else-if="typeof data !== 'undefined' && data.length === 0">repo not exit </p> -->
    <p v-else>Search Repository ...</p>

    <ul>
      <li v-for="lib in data" :key="lib.name">{{ lib.name }}</li>
    </ul>

  </div>
</template>

<script>

import { ref,watchEffect,reactive,toRefs } from "vue";

export default {
  name: 'App',
  setup(){
    const name = ref(null);
    const newName = ref(null);
    /*eslint-disable*/
    const state = reactive({ data: []});


    watchEffect(()=>{
     //console.log("newName", newName.value);
     if(newName.value)
        fetch(`https://api.github.com/users/${newName.value}/repos`)
        .then((response)=>response.json())
        .then((data)=>{
          
          state.data = data;
          console.log("data ->",data,typeof data);
          // name.value;
          name.value = ''

        })
    })


    return {
      name,
      newName,
      ...toRefs(state),

    }
  }
  
}
</script>

<style lang="scss">
// #app {
  // font-family: Avenir, Helvetica, Arial, sans-serif;
  // -webkit-font-smoothing: antialiased;
  // -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  // color: #2c3e50;
  // margin-top: 60px;
// }
</style>
