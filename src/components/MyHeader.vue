<template>
  <div class="todo-header">
    <input
      @keyup.enter="add"
      type="text"
      v-model="title"
      placeholder="請輸入你的任務名稱，按下Enter鍵確認"
    />
  </div>
</template>
<script>
// 載入套件 nanoid
import { nanoid } from "nanoid";
export default {
  name: "MyHeader",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    add() {
      // 校驗數據
      if (!this.title.trim()) return alert("輸入不能為空");
      // 將用戶的輸入包裝成一個 todo object
      const todoObj = {
        id: nanoid(),
        title: this.title,
        done: false,
      };
      // 通知 App 組件去添加一個 todo Object
      this.$emit("addTodo", todoObj);
      // 清空輸入
      this.title = "";
    },
  },
};
</script>
<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
