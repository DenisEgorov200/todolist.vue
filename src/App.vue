<template>
  <div id="app">
    <p>Кол-во невыполненных часов: {{totalHours}}</p>
    <p>Кол-во выполненных часов: {{totalDoneHours}}</p>
    <p>Кол-во выполненных задач на сегодня: {{totalDoneTask}}</p>
    <label>Задача: </label>
    <input type="text" v-model="newTask.titleTask">
    <input type="number" v-model="newTask.hoursTask" v-on:keypress.enter="addTask()">
    <label>Статус:</label>
    <input type="checkbox" v-model="newTask.statusTask">
    <button @click="addTask()" :disabled="newTask.titleTask.length < 1">Добавить</button>
    <hr>
    <div v-for="(element) in task" :key="element.id">
      <input type="checkbox" v-model="element.status">
      <input v-model="element.title" disabled> <!-- :disabled="element.title" -->
      {{element.hours}}
      <button v-on:click="element.show = !element.show">{{element.show ? '+' : '+'}}</button>
      <button @click="deleteTask(element.id)">
        <svg height="10px" viewBox="-40 0 427 427.00131" width="10px" xmlns="http://www.w3.org/2000/svg"><path d="m232.398438 154.703125c-5.523438 0-10 4.476563-10 10v189c0 5.519531 4.476562 10 10 10 5.523437 0 10-4.480469 10-10v-189c0-5.523437-4.476563-10-10-10zm0 0"/><path d="m114.398438 154.703125c-5.523438 0-10 4.476563-10 10v189c0 5.519531 4.476562 10 10 10 5.523437 0 10-4.480469 10-10v-189c0-5.523437-4.476563-10-10-10zm0 0"/><path d="m28.398438 127.121094v246.378906c0 14.5625 5.339843 28.238281 14.667968 38.050781 9.285156 9.839844 22.207032 15.425781 35.730469 15.449219h189.203125c13.527344-.023438 26.449219-5.609375 35.730469-15.449219 9.328125-9.8125 14.667969-23.488281 14.667969-38.050781v-246.378906c18.542968-4.921875 30.558593-22.835938 28.078124-41.863282-2.484374-19.023437-18.691406-33.253906-37.878906-33.257812h-51.199218v-12.5c.058593-10.511719-4.097657-20.605469-11.539063-28.03125-7.441406-7.421875-17.550781-11.5546875-28.0625-11.46875h-88.796875c-10.511719-.0859375-20.621094 4.046875-28.0625 11.46875-7.441406 7.425781-11.597656 17.519531-11.539062 28.03125v12.5h-51.199219c-19.1875.003906-35.394531 14.234375-37.878907 33.257812-2.480468 19.027344 9.535157 36.941407 28.078126 41.863282zm239.601562 279.878906h-189.203125c-17.097656 0-30.398437-14.6875-30.398437-33.5v-245.5h250v245.5c0 18.8125-13.300782 33.5-30.398438 33.5zm-158.601562-367.5c-.066407-5.207031 1.980468-10.21875 5.675781-13.894531 3.691406-3.675781 8.714843-5.695313 13.925781-5.605469h88.796875c5.210937-.089844 10.234375 1.929688 13.925781 5.605469 3.695313 3.671875 5.742188 8.6875 5.675782 13.894531v12.5h-128zm-71.199219 32.5h270.398437c9.941406 0 18 8.058594 18 18s-8.058594 18-18 18h-270.398437c-9.941407 0-18-8.058594-18-18s8.058593-18 18-18zm0 0"/><path d="m173.398438 154.703125c-5.523438 0-10 4.476563-10 10v189c0 5.519531 4.476562 10 10 10 5.523437 0 10-4.480469 10-10v-189c0-5.523437-4.476563-10-10-10zm0 0"/></svg>
      </button>
      <!-- Еще 2 инпута при нажатии на кнопку -->
      <div>
        <input type="text" v-model="element.title" v-show="element.show">
        <input type="number" v-model="element.hours" v-show="element.show" min="0">
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  // Стандартные значения
  data () {
    return {
      newTask: {
        titleTask: '',
        hoursTask: 0,
        statusTask: false,
        doneTask: 1,
        showTask: false,
        msgEditTask: ''
      },
      task: [],
    }
  },
  // Методы
  methods: {
    // Добавление дела
    addTask() {
      this.task.push({
        id: +new Date(),
        title: this.newTask.titleTask,
        hours: this.newTask.hoursTask,
        status: this.newTask.statusTask,
        done: this.newTask.doneTask,
        show: this.newTask.showTask,
        msgEdit: this.newTask.msgEditTask
      });
      this.newTask.titleTask = '';
      this.newTask.hoursTask = 0;
      this.newTask.statusTask = false;
      this.newTask.doneTask = 1;
      this.newTask.msgEditTask = '';
      // this.newTask.showTask = false;
    },
    // Удаление дела
    deleteTask (n) {
      this.task.splice(this.task.findIndex((call) => {
        if (call.id === n) { // Не определяет id, из-за этого не понимает какой именно элемент массива удалять
          return true;
        }
      }), 1)
    },
  },
  // Свойство
  computed: {
    totalHours() {
      let counter = 0;
      this.task.forEach(function(item) {
        item.status ? null : (counter = counter + Number(item.hours));
      });
      return counter;
    },
    totalDoneHours() {
      let counter = 0;
      this.task.forEach(function(item) {
        // Проверка на статус, если активный, то прибавить, иначе null
        item.status ? (counter = counter + Number(item.hours)) : null;
      });
      return counter;
    },
    totalDoneTask() {
      let counter = 0;
      this.task.forEach(function(item) {
        // Проверка на статус, если активный, то прибавить, иначе null
        item.status ? (counter = counter + Number(item.done)) : null;
      });
      return counter;
    },
  },
  // Смотреть
  watch: {
    // Уборка одного минуса, второй не убирается :(
    hoursTask(value) {
      this.newTask.hoursTask = Number(value);
      (value > 0 || value == '') ? this.newTask.hoursTask = value: this.newTask.hoursTask = value * -1;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  outline: none;
  border: none;
  padding: 3px;
}
</style>
