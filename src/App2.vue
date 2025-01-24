<script>
export default {
  data()
  {
    return{
      name:"John Doe",
      status: 'active',
      tasks:['Taskone',"Tasktwo","Taskthree"],
      link:'https://www.google.com'
    }
  },
  methods:{
    toggleStatus(){
     if(this.status==='active')
     {
      this.status='pending'
     }
     else if (this.status==="pending")
     {
      this.status='Inactive'
     }
     else{
      this.status='active'
     }

    }
  }
}
</script>
<template>
  <h1>{{ name }}</h1>
  <p v-if="status==='active'">User is Active</p>
  <p v-else-if="status==='pending'">User is Pending</p>
  <p v-else>User is Inactive</p>
  <h3>
    <ul>
      <li v-for="task in tasks" :key="task">{{ task }}
      </li>
    </ul>
    <a :href="link">Click for google</a>
  </h3>
  <br>
  <button v-on:click="toggleStatus">Change-status</button>
</template>


<script setup>
import { ref ,onMounted} from 'vue';
    const name=ref('John Doe')
    const status =ref('active')
    const tasks =ref(['Task One','Task Three'])
    const newTask = ref(``);
    const toggleStatus=()=>
    {
     if(status.value==='active')
     {
      status.value='pending'
     }
     else if (status.value==="pending")
     {
      status.value='Inactive'
     }
     else{
      status.value='active'
     }
    }
    const addTask= ()=>
    {
      if(newTask.value.trim() !=='')
    {
      tasks.value.push(newTask.value)
      newTask.value=''
    }
    }
    const deleteTask= (index)=>
    {
      tasks.value.splice(index,1)
    }
    onMounted(async()=>
  {
    try
    {
      const resposne = await fetch('https://jsonplaceholder.typicode.com/todos')
      const data = await resposne.json();
      tasks.value= data.map((task)=> task.title)
    }
    catch(error)
    {
      console.log("Error fetching Tasks",error)

    }
  })
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>
  <br />
  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <br />
  <button @click="toggleStatus">Change Status</button>
</template>


