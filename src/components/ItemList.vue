<template>
  <div id="items">
    <ul>
      <li v-for="task in tasksList" :key="task.key">
        <span
          :style="{ textDecoration: task.concluded ? 'line-through' : 'none' }"
          >{{ task.text }}</span
        >
        <button v-if="task.concluded" @click="setDeleteTask(task.key)">Excluir</button>
        <button v-else @click="setCompleteTask(task.key)">Concluir</button>
      </li>
    </ul>
    <div class="footer">
      <button @click="setCleanList()">Limpar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ItemList",
props: ["tasksList", "cleanList", "completeTask", "deleteTask"],
  methods: {
    setCompleteTask(key) {
      this.completeTask(key);
    },
    setDeleteTask(key){
      this.deleteTask(key);
    },
    setCleanList() {
      this.cleanList();
    },
  },
};
</script>

<style scoped>
#items {
  white-space: pre-wrap;
  word-wrap: break-word;
}
ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

ul li {
  padding: 15px 0;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

li:not(:first-child) {
  border-top: 1px solid #eee;
}

button {
  cursor: pointer;
  background: #f3caf0;
  border: 0;
  border-radius: 4px;
  margin-left: 10px;
  padding: 7px 17px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000;
  margin: 10px 0;
}
.footer{
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
