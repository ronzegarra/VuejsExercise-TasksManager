<template lang="pug">
  #app
    section.section
      h1.title Task Management
      nav.nav.has-shadow.formulary
        .container
          input.inputParameter(type="text", placeholder="Enter title" v-model= "newTask.title" )
          input.inputParameter(type="number", placeholder="hours" v-model="newTask.time" )
          .buttons 
            button.button.is-success(@click="addTask") Add Task 
            button.button.is-danger(@click="cancel") Cancel  
            span.totalHours Total Hours: {{totalTime}} hours       
      .container.results(v-if="tasks.length")
        h1.title Task List
        ul
          li.itemList(v-for="t in tasks") {{ t.title }} - {{t.time}} hours 
            button(@click="removeTask(t)") X   
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      name: '',
      tasks: [],
      newTask: {
        title: '',
        time: 0
      }
    }
  },

  created () {
    this.tasks=JSON.parse(window.localStorage.getItem('tasks'))||[]
  },
  
  computed: {
    totalTime () {
      let totalHours = 0
      this.tasks.map(t => { totalHours = totalHours + Number(t.time) })
      return totalHours
    }
  },
  methods: {
    addTask () {
      if (this.newTask.title && this.newTask.time) {
        const task = {
          title: this.newTask.title,
          time: this.newTask.time
        }
        this.tasks.push(task)
        window.localStorage.setItem('tasks', JSON.stringify(this.tasks))
        this.newTask.title = ''
        this.newTask.time = 0
      }
    },
    removeTask (item) {
      let index = this.tasks.indexOf(item)
      this.tasks.splice(index, 1)
    },
    
    cancel () {
      this.newTask.title = ''
      this.newTask.time = 0
    }
  }
}
</script>

<style lang="scss">
  @import './scss/main.scss';
  .results{
    margin-top: 20px
  }
  .formulary{
    margin-top: 20px
  }
</style>
