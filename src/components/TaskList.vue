<template>
  <q-item class="zadatak-zadan">
    <q-item-section>
      <q-checkbox v-model="localTask.completed" label="" :true-value="true" :false-value="false" />
    </q-item-section>

    <q-item-section>
      <div class="task-line">
        <span :class="{ completed: localTask.completed }">{{ localTask.text }}</span>
        <span class="due-date"> | Moraš napraviti do: {{ localTask.dueDate }}</span>
      </div>
    </q-item-section>

    <q-item-section side>
      <q-btn color="red" @click="obrisi">Obriši</q-btn>
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
  data() {
    return {
      localTask: { ...this.task }
    };
  },
  watch: {
    localTask: {
      deep: true,
      handler(newVal) {
        this.$emit("update:task", newVal);
      }
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

.task-line {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}

.due-date {
  font-size: 13px;
  color: rgb(115, 113, 113);
}
</style>
