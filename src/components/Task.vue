<template>
  <div id="task">
    <form @submit.prevent="addTask">
      <input type="text" placeholder="Tarefa de hoje" v-model="task" />
      <button type="submit">Adicionar</button>
    </form>

    <ItemList
      :tasksList="tasksList"
      :cleanList="cleanList"
      :completeTask="completeTask"
      :deleteTask="deleteTask"
    />

    <div class="footer">
      <span v-show="tasksPendentes.length > 0">
        Você tem
        <strong :class="{ pend: pendent }">
          {{ tasksPendentes?.length }}
        </strong>
        tarefas pendentes.
      </span>
    </div>
  </div>
</template>

<script>
import ItemList from "./ItemList.vue";

export default {
  name: "TaskView",
  components: {
    ItemList,
  },
  data() {
    return {
      task: "",
      taskList: [],
      pendent: false,
    };
  },
  methods: {
    addTask() {
      if (this.task !== "") {
        this.tasksList.push({
          text: this.task,
          key: Date.now(),
          concluded: false,
        });
      } else {
        alert("Digite uma tarefa");
        return;
      }
      this.task = "";
    },
    completeTask(key) {
      let index = this.tasksList.findIndex((item) => item.key === key);

      if (index !== -1) {
        this.tasksList[index].concluded = true;
      }
    },
    deleteTask(key) {
      let filter = this.tasksList.filter((item) => {
        return item.key !== key;
      });

      return (this.tasksList = filter);
    },
    cleanList() {
      return (this.tasksList = []);
    },
  },
  watch: {
    tasks: {
      deep: true,
      handler() {
          const filteredConcluded = this.tasksList.filter(
            (task) => !task.concluded
          );
          localStorage.setItem("tasks", JSON.stringify(this.tasksList));
          filteredConcluded.length > 4
            ? (this.pendent = true)
            : (this.pendent = false);
        }
      },
    },
    created() {
      const taskList = localStorage.getItem("tasks");
      this.tasksList = JSON.parse(taskList) || [];
    },
    computed: {
      tasksPendentes() {
        return this.tasksList.filter((task) => !task.concluded);
      },
    },
};
</script>

<style scoped>
#task {
  max-width: 700px;
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  margin: 20px auto;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

form {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
}

form button {
  cursor: pointer;
  background: #f3caf0;
  border: 0;
  border-radius: 4px;
  margin-left: 10px;
  padding: 0 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000;
}

input {
  flex: 1;
  border: 1px solid #eee;
  padding: 6px 10px;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

span {
  font-size: 12px;
  margin-top: 10px;
}
.footer {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: end;
}
.pend {
  color: #ff0000;
}
</style>
