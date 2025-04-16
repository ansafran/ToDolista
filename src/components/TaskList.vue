<template>
  <q-item class="zadatak-zadan">
    <q-item-section>
      
      <q-checkbox 
        :checked="task.completed" 
        label="" 
        :true-value="true" 
        :false-value="false"
        @change="updateCompleted"  
      />
    </q-item-section>

    <q-item-section>
      <span :class="{ completed: task.completed }">{{ task.text }}</span>
      
      <div class="due-date">Moraš napraviti do: {{ task.dueDate }}</div>
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
  methods: {
    obrisi() {
      this.$emit('obrisi', this.index);  
    },
    updateCompleted(value) {
      
      this.$emit('update-task', { index: this.index, completed: value });
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
</style>
