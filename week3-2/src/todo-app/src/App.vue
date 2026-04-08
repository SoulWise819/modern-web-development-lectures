<script setup>
import { InputText, ToggleSwitch } from 'primevue';
import { ref } from 'vue';

// function test() {
//   alert('버튼이 클릭되었습니다!');
// }

const newTodoItem = ref('');
const todoList = ref([]);
const buttonName = ref('등록');

function addTodoList() {
  if (newTodoItem.value.trim() === '') return;
  todoList.value.push({
    text: newTodoItem.value,
    completed: false,
    isEditing: false
  });
  newTodoItem.value = '';
}

function completeTodoClass(item) {
  return item.completed ? 'line-through' : '';
}

function removeTodoList(index) {
  todoList.value.splice(index, 1);
}

function modifyToList(index) {
  todoList.value[index].isEditing = !todoList.value[index].isEditing;
}

</script>

// TODO 목록 리스트를 만들어서 입력창에다가 입력하고 등록 버튼을 누르면
// 변수에 추가되도록 만들어보자

<template>
  <h1 class="text-3xl font-bold underline">
    To-Do List
  </h1>

  <div class="my-4">
    <InputText placeholder="Username" id="UserName" v-model="newTodoItem" @keyup.enter="addTodoList"></InputText>
    <Button @click="addTodoList" class="ml-2">{{ buttonName }}</Button>
  </div>

  <div>
    <ul class="space-y-2">
      <li v-for="(item, index) in todoList" :key="index" class="flex items-center gap-2">
        <ToggleSwitch v-model="item.completed" :binary="true" />

        <template v-if="item.isEditing">
          <InputText v-model="item.text" @keyup.enter="modifyToList(index)" size="small" class="flex-3" />
        </template>
        <template v-else>
          <span :class="completeTodoClass(item)" class="flex-3">
            {{ item.text }}
          </span>
        </template>

        <Button size="small" :icon="item.isEditing ? 'pi pi-check' : 'pi pi-pen-to-square'"
          @click="modifyToList(index)"></Button>
        <Button size="small" icon="pi pi-trash" @click="removeTodoList(index)"></Button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
