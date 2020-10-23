<template>
  <div class="home">
    
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/> -->

    <button v-on:click="foo()">
      ok
    </button>
    <p v-if="visible">Maintenant vous me voyez {{message}}</p>

    <div v-background></div>
    <div v-background="green"></div>

  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
// import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
import todos from '@/components/todos.vue';
import { Background } from '../directives/background.directive';

@Component({
  components: {
    // HelloWorld,
    todos
  },
  directives:{
    Background
  }
})

export default class Home extends Vue {
  message= 'test';
  visible = true;
  
  foo(){
    this.visible = !this.visible;
  }
}
</script>
