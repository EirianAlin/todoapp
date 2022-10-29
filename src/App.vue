<template>
  <h4>Мои задачи</h4>

  <div class="container">

  <div class="wrapper-stat">
    <div class="stat">
      <h5>Срочные</h5>
      <div class="wrapper-progress">
        <div class="bar" :style="{ width: `${statistics.urgent}`}"></div>
      </div>
    </div>
    <div class="stat">
      <h5>Несрочные</h5>
      <div class="wrapper-progress">
        <div class="bar nonUrgant" :style="{ width: `${statistics.nonUrgent}`}"></div>
      </div>
    </div>
  </div>

  <div class="tasks">
    <div
    class="task"
     :class="{ 
      task, 
      checked: task.isChecked }"
     v-for="task, index in tasks"
     :key="task._id"
     @click="checkedHandler(task)"
    >
      <div 
      class="round"
        :class="{
          round,
          urgent: task.type === 'urgent',
          nonUrgent: task.type === 'nonUrgent',
        }"
        ></div>

      <span>{{task.name}}</span>

      <div class="remove-task-icon" @click="removeTask(index)">
        <img src="../src/assets/close.png" />
      </div>
    </div>
  </div>

  <div class="add-task">
    <div class="add-task-input-wrapper">
      <input class="inputForTask" type="text" placeholder="Напишите здесь вашу задачу" v-model="taskName" v-on:keyup.enter="addTask" />
    </div>
    <div class="makeUrgant-wrapper">
    <span class="makeUrgent">Срочная</span>
    <label class="switch">
      <input class="inputForSlider" type="checkbox" v-model="urgent">
      <span class="slider round"></span>
    </label>
    </div>
    <div class="button-wrapper">
    <button @click="addTask">+</button>
    </div>
</div>
</div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  data() {
    return {
      taskName: '',
      urgent: false,
      tasks: [
      {
        _id: "1234",
        name: "Купить продукты",
        isChecked: false,
        type: "nonUrgent",
      },
    ],
  };
 },
 methods: {
  checkedHandler: function (task) {
    task.isChecked = !task.isChecked;
  },

  addTask: function () {
    this.tasks.push({
      _id: Math.random().toString(36).substring(2,7),
      name: this.taskName,
      isChecked: false,
      type: this.urgent ? 'urgent' : 'nonUrgent',
  });
  this.taskName = ''
  },

  removeTask: function (index) {
    delete this.tasks.splice(index, 1)
  },

  makeUrgentTask: function (task) {
    task.type === 'urgent'
  }
 },

 computed: {
  statistics: function() {
    const count = this.tasks.length || 1
    const urgentData = this.tasks.filter(t => t.type === 'urgent')
    const nonUrgentData = this.tasks.filter(t => t.type === 'nonUrgent')

    return {
      urgent: Math.round((urgentData.length * 100) / count) + '%',
      nonUrgent: Math.round((nonUrgentData.length * 100) / count) + '%',
    }
  },
 }
};
</script>



