<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @click="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        :value="todo.title"
        v-show="todo.isEdit"
        @blur="handleBlur(todo, $event)"
        ref="inputTitle"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      @click="handleEdit(todo)"
    >
      編輯
    </button>
  </li>
</template>
<script>
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    handleCheck(id) {
      this.$bus.$emit("checkTodo", id);
    },
    handleDelete(id) {
      if (confirm("確定刪除?")) {
        this.$bus.$emit("deleteTodo", id);
      }
    },
    // 編輯
    handleEdit(todo) {
      if (Object.prototype.hasOwnProperty.call(todo, "isEdit")) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }

      this.$nextTick(function () {
        this.$refs.inputTitle.focus();
      });
    },
    // 失去焦點回調(真正執行修改邏輯)
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) return alert("輸入不得為空");
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
    },
  },
};
</script>
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>
