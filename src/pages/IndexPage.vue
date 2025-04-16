<template>
  <q-page class="flex flex-center">
    <div class="todo-lista">
      <AppLogo :src="logoPath" /> 
      <h1>Tvoja To-Do lista</h1>

      <q-input
        v-model="novi"
        @keyup.enter="dodaj"
        type="text"
        placeholder="Dodaj novi cilj u svoju to-do listu 😊"
        class="zadatak"
        outlined
      />

      <q-input
        v-model="noviDatum"
        type="date"
        label="Do kada želiš to ispuniti?"
        class="zadatak"
        outlined
      />

      <q-btn label="Dodaj" color="primary" @click="dodaj" class="q-mt-sm" />

      <q-list>
        <TaskList
          v-for="(task, index) in tasks"
          :key="index"
          :task="task"
          :index="index"
          @obrisi="obrisi" 
          @update-task="updateTask"  
        />
      </q-list>
    </div>
  </q-page>
</template>

<script>
import AppLogo from '../components/AppLogo.vue'; 
import TaskList from '../components/TaskList.vue';

export default {
  components: {
    AppLogo,  
    TaskList
  },
  data() {
    return {
      novi: "",
      noviDatum: "",
      tasks: [],
      logoPath: '/logo.png',
    };
  },
  mounted() {
    setTimeout(() => {
      this.logoPath = '/done.png';  // Test promjena loga nakon 3 sekunde jel svge oke
    }, 3000);
  },
  methods: {
    dodaj() {           
      if (this.novi.trim()) {
        const danas = new Date();
        const defaultni = new Date(danas);
        defaultni.setDate(defaultni.getDate() + 3); // Defaultni datum za 3 dana ako ne unesemo datum
        const datumFormat = this.formatDate(this.noviDatum || defaultni);
        
        this.tasks.push({
          text: this.novi,
          completed: false,
          dueDate: datumFormat
        });

        this.novi = "";
        this.noviDatum = "";
      }
    },
    obrisi(index) {
      this.tasks.splice(index, 1);  
    },
    updateTask({ index, completed }) {
      
      this.tasks[index].completed = completed;
    },
    formatDate(date) {
      const d = new Date(date);
      const day = ("0" + d.getDate()).slice(-2); 
      const month = ("0" + (d.getMonth() + 1)).slice(-2); 
      const year = d.getFullYear();
      return `${day}/${month}/${year}`;
    }
  },
};
</script>
