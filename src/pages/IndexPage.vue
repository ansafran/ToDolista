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
          v-model:task="tasks[index]"
          :index="index"
          @obrisi="obrisi"
        />
      </q-list>
    </div>
  </q-page>
</template>

<script>
import Logo from '../components/AppLogo.vue';
import TaskList from '../components/TaskList.vue';

export default {
  components: {
    Logo,
    TaskList
  },
  data() {
    return {
      novi: "",
      noviDatum: "",
      tasks: [],
      logoPath: '/logo.png'
    };
  },
  mounted() {
    setTimeout(() => {
      this.logoPath = '/done.png';
    }, 3000);
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
          dueDate: datumFormat
        });

        this.novi = "";
        this.noviDatum = "";
      }
    },
    obrisi(index) {
      this.tasks.splice(index, 1);
    },
    formatDate(date) {
      const d = new Date(date);
      const day = ("0" + d.getDate()).slice(-2);
      const month = ("0" + (d.getMonth() + 1)).slice(-2);
      const year = d.getFullYear();
      return `${day}/${month}/${year}`;
    }
  }
};
</script>

<style scoped>
html, body {
  height: 100%;
  margin: 0;
  overflow: hidden;
}

.todo-lista {
  max-width: 600px;
  margin: 0;
  text-align: center;
}

.zadatak {
  margin-bottom: 20px;
  font-size: 17px;
}

.h1 {
  display: inline;
  font-size: 24px;
  margin: 0;
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
</style>
