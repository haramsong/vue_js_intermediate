<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodoItem="addOneItem"></TodoInput>
    <TodoList 
      :propsdata="todoItems" 
      @removeTodoItem="removeOneItem" 
      @toggleTodoItem="toggleOneItem">
    </TodoList>
    <TodoFooter @clearAll="clearAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data: function() {
    return {
      todoItems: [],
    }
  },
  methods: {
    addOneItem: function(todoItem) {
      var obj = {completed: false, item: todoItem};
      // 저장하는 로직
      // 객체를 넣을 때는 JSON.stringfy()를 통해 문자열 형태를 넣어야 data 육안으로 확인 가능
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index) {
      // console.log(todoItem, index);
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem: function(todoItem, index) {
      // = todoItem.completed = !todoItem.completed; 컴포넌트 관계를 더 명확하게 함.
      this.todoItems[index].completed = !this.todoItems[index].completed
      // 로컬 스토리지의 데이터 갱신
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function() {
      localStorage.clear();
      this.todoItems = [];
    },
  },
  created: function() {
    if (localStorage.length > 0) {
      for (var i = 0 ; i < localStorage.length ; i ++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
          // this.todoItems.push(localStorage.key(i));
        }
      }
    }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter,
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #F6F6F6;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
