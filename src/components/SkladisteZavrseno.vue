<template>
    <div>
      
      <q-badge color="green" align="top right" text-color="black" class="izvrseno" >
        {{ completedTasks.length }} - Toji završeni zadaci 💪
      </q-badge>
  
    <q-list>
    <q-item v-for="(task, index) in completedTasks" :key="index">
          <q-item-section>
             <div>
                {{ index + 1 }}. {{ task.text }}
                </div>
         <div v-if="task.izvrsenoDana" class="prioritet">
                ✅  Napravljeno: {{ task.izvrsenoDana }}
            </div>
         </q-item-section>
        </q-item>
      </q-list>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SkladisteZavrseno',
    props: {
      completedTasks: {
        type: Array,
        required: true
      }
    }
  }
  </script>
  
  <style scoped>
  .due-date {
    font-size: 12px;
    color: rgb(0, 0, 0);
    margin-top: 5px;
  }
  </style>
  