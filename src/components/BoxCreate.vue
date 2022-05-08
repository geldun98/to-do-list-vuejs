<template>
  <div class="box-create">
    <h3 class="box-title">CREATE NEW TASK</h3>
    <div class="box-nav">
      <div class="nav-item">
        <input type="radio" v-model="activeTodo" value="todo" />
        <span>TO DO</span>
      </div>
      <div class="nav-item">
        <input type="radio" v-model="activeTodo" value="inprogress" />
        <span>IN PROGRESS</span>
      </div>
      <div class="nav-item">
        <input type="radio" v-model="activeTodo" value="done" />
        <span>DONE</span>
      </div>
    </div>
    <input
      v-model="valueInput"
      type="text"
      class="box-input"
      placeholder="Enter your task ...."
    />
    <div class="box-button">
      <button class="btn-save" @click="handleSave">SAVE</button>
      <button class="btn-cancel" @click="handleCancel">CANCEL</button>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "box-create",
  props: {
    active: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      activeTodo: this.active,
      valueInput: "",
    };
  },
  methods: {
    handleCancel() {
      this.$emit("eventModal");
    },
    handleSave() {
      if (this.valueInput.length > 0) {
        this.$emit(
          "eventInput",
          {
            data: this.valueInput,
            date: `${moment().format("l")}, ${moment().format("LT")}`,
          },
          this.activeTodo
        );
        this.handleCancel();
      } else {
        console.log("Please enter your todo");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.box-create {
  width: 350px;
  min-height: 200px;
  background: #fff;
  transform: translate(-50%, -50%);
  top: 35%;
  left: 50%;
  position: relative;
  padding: 15px;
}
.box-title {
  color: #5680f9;
  padding-bottom: 10px;
  border-bottom: 1px solid #ccc;
}
.box-nav {
  padding-top: 15px;
  display: flex;
  justify-content: space-between;
}
.nav-item {
  input {
    border-radius: 100%;
  }
  span {
    margin-left: 5px;
    font-weight: 600;
  }
}
.box-input {
  margin-top: 15px;
  font-size: 15px;
  padding: 5px 10px;
  width: 100%;
  outline-color: #5680f9;
}
.box-button {
  padding-top: 20px;
  display: flex;
  justify-content: flex-end;
  button {
    outline: none;
    border: 0;
    padding: 5px 10px;
    cursor: pointer;
  }
  .btn-save {
    background: #5680f9;
    color: white;
  }
  .btn-cancel {
    margin-left: 15px;
    background: white;
    border: 1px solid rgb(80, 80, 80);
    color: rgb(80, 80, 80);
  }
}
</style>
