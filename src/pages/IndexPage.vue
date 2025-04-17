<template>
  <q-page class="flex flex-center">
    <div class="todo-lista">
      <Logo :src="logoPath" />
      <h1>Tvoja To-Do lista</h1>

      <q-input
        v-model="novi"
        @keyup.enter="dodaj"
        type="text"
        placeholder="Dodaj novi cilj u svoju to-do listu 😊"
        class="zadatak"
        outlined
      />

      <q-btn
        dense
        flat
        icon="add"
        label="Dodatne opcije"
        @click="pokaziOpcije = !pokaziOpcije"
        class="dodaj"
      />

      <div v-show="pokaziOpcije" class="dodatno">
        <q-input
          v-model="opis"
          type="textarea"
          label="Opis"
          outlined
          class="dodaj"
        />

        <q-select
          v-model="prioritet"
          :options="['Visok', 'Srednji', 'Nizak']"
          label="Prioritet"
          outlined
          class="dodaj"
        />
      </div>

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
          @toggle-completed="toggleCompleted"
        />
      </q-list>

      <SkladisteZavrseno
        :completedTasks="completedTasks"
      />
    </div>
  </q-page>
</template>

<script>
import Logo from '../components/AppLogo.vue';
import TaskList from '../components/TaskList.vue';
import SkladisteZavrseno from '../components/SkladisteZavrseno.vue';

export default {
  components: {
    Logo,
    TaskList,
    SkladisteZavrseno,
  },
  data() {
    return {
      novi: "",
      noviDatum: "",
      opis: "",
      prioritet: "",
      pokaziOpcije: false,
      tasks: [],
      completedTasks: [],
      logoPath: '/logo.png',
    };
  },
  methods: {
    dodaj() {
      if (this.novi.trim()) {
        const danas = new Date();
        const defaultni = new Date(danas);
        defaultni.setDate(defaultni.getDate() + 3);
        const datumFormat = this.formatDate(this.noviDatum || defaultni);

        this.tasks.push({
          text: this.novi,
          completed: false,
          dueDate: datumFormat,
          opis: this.opis,
          prioritet: this.prioritet,
          izvrsenoDana: null // jer inace pokazuje datum pocetni kao
        });

        this.novi = "";
        this.noviDatum = "";
        this.opis = "";
        this.prioritet = "";
        this.pokaziOpcije = false;
      }
    },
    obrisi(index) {
      this.tasks.splice(index, 1);
    },
    toggleCompleted(index) {
      const task = this.tasks[index];

      task.completed = !task.completed;

      if (task.completed) {
    
        const danas = new Date();
        const dan = ("0" + danas.getDate()).slice(-2);
        const mjesec = ("0" + (danas.getMonth() + 1)).slice(-2);
        const godina = danas.getFullYear();
        task.izvrsenoDana = `${dan}/${mjesec}/${godina}`;

        // Za premjestit zadatak kad se klikne checkbox zavrseno u moje skladiste
        this.completedTasks.push(task); //makni s popisa todo
      } else {
        task.izvrsenoDana = null;
        
        
        this.tasks.push(task);
        this.completedTasks.splice(index, 1); 
      }
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

<style scoped>
.todo-lista {
  max-width: 600px;
  margin: 0;
  text-align: center;
}

.zadatak {
  margin-bottom: 20px;
  font-size: 17px;
}

.q-page {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  height: 100%;
  overflow-y: auto;
}

.q-list {
  margin-top: 20px;
  flex-grow: 1;
}

.dodatno {
  display: flex;
  flex-direction: column;
}
</style>
