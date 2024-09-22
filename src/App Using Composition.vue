<script setup>
import { ref } from 'vue'; // for wrapping up variable 
import { onMounted } from 'vue';
/*
export default{ // Compostition API
  setup () {
    const name = ref('Muhammad Ahmad');
    const status = ref('active');
    const tasks = ref(['Task One','Task Two','Task Three']);

    const toggleStatus = () => {
      if (status.value === 'active') {
        status.value = 'pending';
      } else if (status.value === 'pending') {
        status.value = 'inactive';
      } else {
        status.value = 'active';
      }
    }
    return {
    name,
    status,
    tasks,
    toggleStatus
  }
  }, 
};*/

  const name = ref('Muhammad Ahmad');
  const status = ref('active');
  const tasks = ref(['Task One','Task Two','Task Three']);
  const newTask = ref('');

  const toggleStatus = () => {
    if (status.value === 'active') {
      status.value = 'pending';
    } else if (status.value === 'pending') {
      status.value = 'inactive';
    } else {
      status.value = 'active';
    }
  };

  const addTask = () => {
    if (newTask.value.trim !== '') {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  };

  const DeleteTask = (index) => {
    tasks.value.splice(index,1);
  };

  onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title); 
    } catch (error) {
      console.log('Error Fetching Task');
    }

  });
</script>

<template>
  <h1>{{ name }}</h1> 
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is inactive</p>
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task </label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="(task,index) in tasks":key="task">
      <span>
        {{ task }}
      </span>
      <button @click="DeleteTask(index)">Del</button>
    </li>
  </ul>

  <!-- <button v-on:click="toggleStatus">Click me</button> -->
  <button @click="toggleStatus">Change status</button>
</template>
