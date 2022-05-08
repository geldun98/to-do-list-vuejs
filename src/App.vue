<template>
  <div id="app">
    <div class="content">
      <h1>TO DO LIST</h1>
      <div class="box-list">
        <box
          title="TO DO"
          @eventModal="changeModal"
          active="todo"
          @eventActive="handleActive"
          :arrayValue="arrayTodo"
          @eventDelete="handleDelete"
          @eventEdit="handleEdit"
        ></box>
        <box
          title="IN PROGRESS"
          @eventModal="changeModal"
          active="inprogress"
          @eventActive="handleActive"
          :arrayValue="arrayInProgress"
          @eventDelete="handleDelete"
          @eventEdit="handleEdit"
        ></box>
        <box
          title="DONE"
          @eventModal="changeModal"
          active="done"
          @eventActive="handleActive"
          :arrayValue="arrayDone"
          @eventDelete="handleDelete"
          @eventEdit="handleEdit"
        ></box>
      </div>
    </div>
    <div v-if="isModal" class="modal">
      <box-create
        @eventModal="changeModal"
        :active="nameActive"
        @eventInput="handleInput"
      ></box-create>
    </div>
    <notice v-if="isNotice"></notice>
  </div>
</template>

<script>
import Box from "./components/Box.vue";
import BoxCreate from "./components/BoxCreate.vue";
import Notice from "./components/Notice.vue";
export default {
  name: "App",
  components: { Box, BoxCreate, Notice },
  data() {
    return {
      isModal: false,
      nameActive: "",
      arrayTodo: [],
      arrayInProgress: [],
      arrayDone: [],
      isNotice: false,
    };
  },
  watch: {
    totalData() {
      localStorage.localData = JSON.stringify(this.totalData);
    },
  },
  computed: {
    totalData() {
      return {
        todo: this.arrayTodo,
        inprogress: this.arrayInProgress,
        done: this.arrayDone,
      };
    },
  },
  mounted() {
    if (localStorage.localData) {
      const dataLocal = JSON.parse(localStorage.localData);
      this.arrayTodo = dataLocal.todo;
      this.arrayInProgress = dataLocal.inprogress;
      this.arrayDone = dataLocal.done;
    }
  },
  methods: {
    changeModal() {
      this.isModal = !this.isModal;
    },
    handleActive(name) {
      this.nameActive = name;
    },
    handleInput(value, name) {
      if (name === "todo") {
        this.arrayTodo = [...this.arrayTodo, value];
      }
      if (name === "inprogress") {
        this.arrayInProgress = [...this.arrayInProgress, value];
      }
      if (name === "done") {
        this.arrayDone = [...this.arrayDone, value];
      }
    },
    handleDelete(id, name) {
      if (confirm("Are your sure to delete this task?")) {
        this.isNotice = true;
        setTimeout(() => {
          this.isNotice = false;
        }, 1000);
        if (name === "todo") {
          const cloneArrayValue = [...this.arrayTodo];
          cloneArrayValue.splice(id, 1);
          this.arrayTodo = cloneArrayValue;
        }
        if (name === "inprogress") {
          const cloneArrayValue = [...this.arrayInProgress];
          cloneArrayValue.splice(id, 1);
          this.arrayInProgress = cloneArrayValue;
        }
        if (name === "done") {
          const cloneArrayValue = [...this.arrayDone];
          cloneArrayValue.splice(id, 1);
          this.arrayDone = cloneArrayValue;
        }
      } else {
        return;
      }
    },
    handleEdit(id, value, name) {
      if (name === "todo") {
        const cloneData = [...this.arrayTodo];
        cloneData[id] = { ...cloneData[id], ...value };
        this.arrayTodo = cloneData;
      }
      if (name === "inprogress") {
        const cloneData = [...this.arrayInProgress];
        cloneData[id] = { ...cloneData[id], ...value };
        this.arrayInProgress = cloneData;
      }
      if (name === "done") {
        const cloneData = [...this.arrayDone];
        cloneData[id] = { ...cloneData[id], ...value };
        this.arrayDone = cloneData;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  position: relative;
  h1 {
    padding-top: 35px;
    font-size: 25px;
    text-align: center;
    margin-bottom: 24px;
    font-weight: 700;
    color: #5680f9;
  }
  .box-list {
    display: flex;
    justify-content: space-evenly;
  }
  position: relative;
  .modal {
    position: fixed;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.342);
    top: 0;
    left: 0;
    z-index: 10;
  }
}
</style>
