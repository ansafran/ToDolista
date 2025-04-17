<template>
  <q-item class="zadatak-zadan">
    <div v-if="task.izvrsenoDana" class="izvrseno-datum">
  ✅ Završeno: {{ task.izvrsenoDana }}
  </div>

    <q-item-section>
      <q-checkbox
        :model-value="task.completed"
        @update:model-value="$emit('toggle-completed', index)"
      />
    </q-item-section>
    <q-item-section>


 <div :class="{ completed: task.completed }">
        {{ task.text }}
      </div>

       <div class="due-date">
        Moraš napraviti do: {{ task.dueDate }}
      </div>

      <div v-if="task.opis" class="opis">
        Opis: {{ task.opis }}
      </div>

      <q-badge
        v-if="task.prioritet"
      :color="task.prioritet === 'Visok' ? 'red' : task.prioritet === 'Srednji' ? 'orange' : 'green'"
        class="prioritet"
      >
        {{ task.prioritet }}
      </q-badge>
    </q-item-section>

    <q-item-section side>
      <q-btn color="red" icon="delete" @click="obrisi" flat />
 </q-item-section>
  </q-item>
</template>

<script>
export default {
  name: "TaskList",
  props: {
    task: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  methods: {
    obrisi() {
      this.$emit("obrisi", this.index);
    }
  }
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
.zadatak-zadan {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}
.due-date {
  font-size: 12px;
  color: rgb(115, 113, 113);
  margin-top: 5px;
}
.opis {
  font-size: 13px;
  color: #444;
  margin-top: 3px;
}
.izvrseno-datum {
  font-size: 12px;
  color: green;
  margin-top: 5px;
}

</style>
