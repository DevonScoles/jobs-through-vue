<script setup>
import { onMounted, ref } from 'vue';

const name = ref('John Doe'); //ref allows for the variable to be changed later on
const status = ref('active');
const tasks = ref(['Task one', 'Task two', 'Task three']);
const newTask = ref('Yallah habibi');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  } else if (status.value === 'pending'){
    status.value = 'inactive';
  } else{
    status.value = 'active';
  }
};

const addTask = () => {
  if (newTask.value.trim() != '') {
    tasks.value.unshift(newTask.value);
    newTask.value = '';
  }
};
const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try{
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title) 
    //when we call data it will return the objects known as todos. so we use map in order to
    // return an array of tasks and for each (task) we return the task.title since we only want the title 
    
  } catch (error) {
      console.log('Error fetching tasks')
  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <br>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p><br>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task: </label>
    <input type="text" name="newTask" id="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>
  
<br><h3>Tasks:</h3><br>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
      {{ task }}
      </span>
      <button class="btn" @click="deleteTask(index)" >delete</button>
    </li>
  </ul>

<br>
  <!--   v-on:click="" is the same as @click=""   -->
  <button @click="toggleStatus">Change Status</button>

</template>

<style>
body {
    margin: 0;
    padding: 0;
    text-align: start;
}


.btn {
    cursor: pointer;
    border: transparent;
    background-color: transparent;
    color: #ac0101;
    font-size: 1em;
}
</style>