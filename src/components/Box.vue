<template>
  <div class="box">
    <div class="box-top">
      <div class="current">
        <span class="current-number">{{ arrayValue.length }}</span>
        <span class="current-text">{{ title }}</span>
      </div>
      <button class="btn-add" @click="handleCreateTask">New task</button>
    </div>
    <div class="box-content">
      <div v-for="(item, index) in arrayValue" :key="index" class="box-item">
        <div class="box-date">
          <i class="fa-solid fa-calendar-days"></i>
          <span> {{ item.date }}</span>
        </div>
        <div class="box-detail">
          {{ item.data }}
          <div class="box-btn">
            <button class="btn-edit btn" @click="handleEdit">
              <i :id="index" class="fa-solid fa-pen-to-square"></i>
            </button>
            <button class="btn-delete btn" @click="handleDelete">
              <i :id="index" class="fa-solid fa-trash-can"></i>
            </button>
          </div>
          <div class="box-edit" v-show="index === idEdit">
            <input type="text" v-model="valueEdit" />
            <div class="box-edit-btn">
              <button class="btn btn-ok" @click="handleSaveEdit">
                <i class="fa-solid fa-check"></i>
              </button>
              <button class="btn btn-cancel" @click="handleCancelEdit">
                <i class="fa-solid fa-ban"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "box-comp",
  props: {
    title: {
      type: String,
      default: "",
    },
    active: {
      type: String,
      default: "",
    },
    arrayValue: {
      type: Array,
    },
  },
  data() {
    return {
      idEdit: "",
      valueEdit: "",
    };
  },
  methods: {
    handleCreateTask() {
      this.$emit("eventModal");
      this.$emit("eventActive", this.active);
    },
    handleDelete(event) {
      this.$emit("eventDelete", event.target.id, this.active);
    },
    handleEdit(event) {
      this.idEdit = +event.target.id;
      this.valueEdit = this.arrayValue[+event.target.id].data;
    },
    handleSaveEdit() {
      this.$emit(
        "eventEdit",
        this.idEdit,
        { data: this.valueEdit },
        this.active
      );
      this.idEdit = "";
    },
    handleCancelEdit() {
      this.idEdit = "";
    },
  },
};
</script>

<style lang="scss" scoped>
.box {
  width: 365px;
  min-height: 375px;
  background: #ecf0f1;
  padding: 20px;
}
.current {
  display: flex;
  align-items: center;
  &-number {
    font-size: 25px;
    color: white;
    width: 35px;
    height: 35px;
    background: #3498db;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    border-radius: 100%;
  }
  &-text {
    font-weight: 600;
    margin-left: 10px;
  }
}
.btn-add {
  color: white;
  font-size: 20px;
  background: #3498db;
  outline: none;
  border: 0;
  padding: 5px 10px;
  cursor: pointer;
}
.box-top {
  display: flex;
  justify-content: space-between;
}
.box-item {
  background: white;
  margin-top: 15px;
  font-size: 20px;
  padding: 5px 10px;
  display: flex;
  justify-content: space-between;
}
.btn {
  border: 0;
  outline: none;
  color: white;
  padding: 3px 5px;
  cursor: pointer;
  background: white;
  &-delete {
    i {
      color: red;
      font-size: 16px;
    }
  }
  &-edit {
    margin-right: 15px;
    i {
      color: blue;
      font-size: 16px;
    }
  }
  &-ok {
    i {
      color: green;
      font-size: 16px;
    }
  }
  &-cancel {
    i {
      color: gray;
      font-size: 16px;
    }
    margin-left: 15px;
  }
}
.box-item {
  position: relative;
  display: flex;
  flex-direction: column;
}
.box-edit {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: white;
  display: flex;
  align-items: center;
  padding: 5px 10px;
  justify-content: space-between;
}
.box-detail {
  display: flex;
  justify-content: space-between;
}
.box-date {
  color: gray;
  font-size: 14px;
  display: flex;
  align-items: center;
  span {
    margin-left: 15px;
  }
  margin-bottom: 10px;
}
</style>
