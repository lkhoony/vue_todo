<template>
  <div id="app" class = "container">
    <h1 class = "text-center mb-3">Todo App</h1>
    <Clock />
    <!-- v-model : 데이터 바인딩 -->
    <!-- @event : 이벤트 시 작동 되는 함수 -->
    <Input 
      @add-todo='addTodo'/>
    
    <CompletedTodo 
      :todos = "todos"
    />
    <hr>
    <!-- 리스트 렌더링 되는 component는 항상 key 라는 props 값이 필요 -->
    <!-- 가상돔에서 리스트 component에서 변경된 부분을 감지할 때 key라는 값을 이용하기 때문에 항상 필요 -->
    <!-- 컴포넌트를 사용할 때 key를 등록 -->
    <TodoList 
      :todos="todos"
      @toggle-checkbox = "toggleCheckbox"
      @click-delete = "deleteTodo"/>
  </div>
</template>

<script>

import CompletedTodo from '@/components/CompletedTodo.vue'
import Clock from '@/components/Clock.vue'
import Input from '@/components/Input.vue'
import TodoList from '@/components/TodoList.vue'
export default {

  components : {
    CompletedTodo,
    Clock,
    Input,
    TodoList
  },

  data() {
    return {

      todos : [
        {id : 1, text : 'buy a car', checked :  false, completed : ''},
        {id : 2, text : 'play game', checked :  false, completed : ''}        
      ],

      day : ['Sun', 'Mon', 'Tue', 'Wed', 'Tur', 'Fri', 'Sat'],

      Month : ['January', 'February', 'March', 'April', 'May',
              'June', 'July', 'August', 'September', 'October',
              'November', 'December']

    }
  },

  methods : {
    addTodo(todoText){
      this.todos.push({
        id : Math.random(),
        text : todoText,
        checked : false,
        completed : ''
      });
    },

    // {} : 객체에 바로 접근 가능
    toggleCheckbox({id, checked}){
      const index = this.todos.findIndex(todo => {
        return todo.id === id;
      });
      this.todos[index].checked = checked;

      const date = new Date();

      if(checked){
        this.todos[index].completed = "completed at " + date.toLocaleTimeString();
      }else{
        this.todos[index].completed = '';
      } 
    },

    deleteTodo(id){
      const index = this.todos.findIndex(todo => {
        return todo.id === id;
      });
      // splice(시작위치, 제거건수)
      this.todos.splice(index,1);

      // # filter를 사용해서도 가능
      // this.todos = this.todos.filter(todo => todo.id!== id)
    }

  }
}
</script>
