<script setup>
// vue3 컴포넌트 api
import { reactive, computed } from 'vue';

// reactive함수: 객체를 반응성으로 만들며 값이 변경되면 리렌더링이 됨
const data = reactive({
  newItem: '',
  items: [],
});

//computed함수: data.itmes가 변경될때만 함수가 실행
const totalItems = computed(() => data.items.length);
// 할 일 완료 여부
const isComplete = computed(() => data.items.filter((item) => item.completed).length);

// 할 일 추가, 함수표현식
const addItem = () => {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
};
const deleteItem = (id) => {
  const itemToDelete = data.items.find((item) => item.id === id);

  let index = data.items.indexOf(itemToDelete);
  console.log(index);

  data.items.splice(index, 1);
};
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">완료: {{ isComplete }} / 할 일: {{ totalItems }}</div>
    <div class="todo_add">
      <!-- v-model 디렉티브로 폼요소와 데이터를 양방향 연결 -->
      <input
        type="text"
        v-on:keyup.enter="addItem()"
        v-model="data.newItem"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
      />
      <button class="add_btn" v-on:click="addItem()" type="button">Add</button>
    </div>
    <ul class="todo_list">
      <!-- 리스트마다 고유 id를 key속성에 단방향 연결 -->
      <li v-for="(item, index) in data.items" v-bind:key="item.id" v-bind:class="{ completed: item.completed }">
        <!-- 변수데이터와 속성연결시 v-bind사용 -->
        <!-- 라벨클릭시 for로 연결된 체크박스가 클릭되며
        true, false가 발생하며 v-model로 연결된 속성에 들어감 -->
        <input v-bind:id="`check${item.id}`" v-model="item.completed" type="checkbox" />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button v-on:click="deleteItem(item.id)" class="remove_btn" type="button">Remove</button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 40px 20px;
}

.app h1 {
  font-size: 30px;
  font-weight: 700;
  color: var(--text-color-900);
  margin-bottom: 20px;
}

.todo_count {
  margin: 10px 0;
}

.todo_add input[type='text'] {
  height: 40px;
  border: 1px solid #ddd;
  padding: 0 10px;
  width: calc(100% - 60px);
  border-radius: 4px;
  margin-right: 10px;
}
.todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  background: #333;
  color: var(--text-color-400);
  border: none;
  border-radius: 4px;
}

.todo_list {
  margin-top: 20px;
}
.todo_list label {
  flex-grow: 1;
  line-height: 32px;
}
.todo_list li {
  margin-bottom: 10px;
  display: flex;
}
.todo_list li.completed label {
  color: #ccc;
  text-decoration: line-through;
}
.remove_btn {
  background: none;
  height: 32px;
  border: 1px solid var(--primary-color);
  padding: 0 5px;
  color: var(--primary-color);
  border-radius: 4px;
  margin-left: 20px;
}
</style>
