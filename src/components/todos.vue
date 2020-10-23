<template>
  <div class="todos">

    <h1>todos Component</h1>

    <div>
      <input v-model="newTask" placeholder="new task">
      <button v-on:click="add()">add</button>
      <button v-on:click="supp(newTask)">supp</button>
    </div>
    <br>

    <div>
      <select v-model="selected">
        <option disabled value="">Select a finished task</option>
        <option v-for="task in tasks" v-bind:key="task.name">
          {{task.name}}
        </option>
      </select>
      <button v-on:click="finish()">Finish</button>
      <button v-on:click="supp(selected)">supp</button>
      <button v-on:click="modify()">modify</button>
      <span v-if="this.modifyBool">
        <input type="text" v-model="modifyTaskName">
        <button v-on:click="modifyV(true)">OK</button>
        <button v-on:click="modifyV(false)">Cancel</button>
      </span>
      <br>
      <!-- <span>Selected: {{ selected }}</span> -->
    </div>
    <br>

    <div>
      <button v-on:click="delAllTicked()">Del All Ticked</button>
      <button v-on:click="delFinishedTask()">Del Finished Task</button>
      <br>
      Filter :
      <select v-model="filter" @change="search()">
        <option value="">all</option>
        <option>aFaire</option>
        <option>Fait</option>
        <option>Clean</option>
      </select>
      <!-- <span>Filter: {{ filter }}</span> -->
      <br>
      TodoList : 
      <footer v-if="this.notFinishedTask() > 0">
        {{this.notFinishedTask()}} Todo left
      </footer>
      <br>
      <li v-for="task in todoList" v-bind:key="task.name">
        <button v-on:click="task.toggle(), search()">
          <span v-if="task.bool">X</span>
          <span v-if="!task.bool">_</span>
        </button>
        <a :href ="task.href">{{task.name}}</a> => {{ task.state }}
      </li>
      <br>
    </div>


  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

  class Task {
    constructor(name: string){
      this.name = name
      this.state = 'aFaire'
    }
    name!: string;
    state: string;
    href = '';
    bool = false;

    toggle(){this.bool = !this.bool}
  }

  @Component
  export default class Todos extends Vue {
    newTask = '';
    selected = '';
    tasks: Task[]=[];
    filter = '';
    todoList = this.tasks;
    modifyBool = false;
    modifyTaskName = '';

    modify(){
      this.modifyBool = true
    }

    modifyV(b: boolean){
      if (b) {
        if (this.modifyTaskName != '' && !this.tasks.find(t => t.name == this.modifyTaskName)){
          this.tasks.filter(t => t.name == this.selected)
            .map(t => t.name = this.modifyTaskName)
          this.modifyBool = false
        }
      }else{
        this.modifyBool = false
      }
    }
    
    notFinishedTask(): number {
      return this.tasks.filter(t => t.state != 'Fait').length
    }

    delAllTicked(){
      this.tasks = this.tasks.filter(t => !t.bool)
      this.search()
    }

    delFinishedTask(){
      this.tasks = this.tasks.filter(t => t.state != 'Fait')
      this.search()
    }

    add(): void {
      if (this.newTask != '' && !this.tasks.find(t => t.name == this.newTask)){
        this.tasks.push(new Task(this.newTask))
        this.newTask = ''
        this.search()
      }
    }

    supp(supp: string): void {
      if (supp != '' && this.tasks.find(t => t.name == supp)){
        this.tasks = this.tasks.filter(t => t.name != supp)
        this.search()
      }
    }

    finish(): void {
      if (this.selected != ''){
        const sel = this.tasks.find(t => t.name == this.selected);
        if (sel) {
          sel.state = 'Fait'
          this.selected = ''
        }
      }
    }

    search(): void {
      if (this.filter != '') {
        this.todoList = this.tasks.filter(t => t.state == this.filter)
      }else{
        this.todoList = this.tasks
      }
    }
    
  }

</script>

<style scoped>

</style>
