<template>
  <div class="hello">
    <h1>{{ msg }}</h1>    
    <h2>{{this.msg1}}</h2>
    <button v-on:click ="foo()">ok</button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  private msg1 = 'test';
  
  foo(){
    this.msg1='titi';
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
