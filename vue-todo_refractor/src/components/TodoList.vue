<template>
    <div>
      <transition-group name="list" tag="ul">
        <li v-for="(todoItem, index) in this.storedTodoItems" 
            :key="todoItem.item" 
            class="shadow">
          <i class="checkBtn fas fa-check" 
            :class="{checkBtnCompleted: todoItem.completed}" 
            @click="toggleComplete({todoItem, index})">
          </i>
          <span :class="{textCompleted: todoItem.completed}">
            {{ todoItem.item }}
          </span>
          <span class="removeBtn" 
                @click="removeTodo({todoItem, index})">
            <i class="fas fa-trash-alt" />
          </span>
        </li>
      </transition-group>
    </div>
  </template>
  
  <script>
  import { mapGetters, mapMutations } from 'vuex'

  export default {
    methods: {
      // 넘겨줄 때 인자를 가지고 있으면 mutation에서 같이 넘겨주지만 넘기는 인자가 두개면 하나의 객체로 만들어서 보내줘야 함.
      ...mapMutations({
        removeTodo: 'removeOneItem',
        toggleComplete: 'toggleOneItem',
      }),
      // removeTodo(todoItem, index) {
      //   this.$store.commit('removeOneItem', { todoItem, index });
      // },
      // toggleComplete(todoItem, index) {
      //   this.$store.commit('toggleOneItem', { todoItem, index });
      // },
    },
    computed: {
      // todoItems() {
      //   return this.$store.getters.storedTodoItems;
      // }
      ...mapGetters(['storedTodoItems'])
    }
  }
  </script>
  
  <style scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }
  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  .checkBtnCompleted {
    color: #b3adad;
  }
  .textCompleted {
    text-decoration: line-through;
    color: #de4343;
  }
  .removeBtn {
    margin-left: auto;
    color: #de4343;
  }
  .list-item {
    display: inline-block;
    margin-right: 10px;
  }
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
  </style>