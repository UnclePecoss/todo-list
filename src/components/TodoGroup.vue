<script setup lang="ts">
import { TodoStatus } from '@/types';
import useTodos from '@/Store/useTodos.ts';
import draggable from 'vuedraggable';
import CreateTodo from '@/components/CreateTodo.vue'
interface Props {
  status: TodoStatus;
}

const props = defineProps<Props>();

const { getTodosByStatus, deleteTodo, updateTodo } = useTodos();
const todoList = getTodosByStatus(props.status);

const groupLabel = {
    [TodoStatus.Pending]: 'Pending',
    [TodoStatus.InProgress]: 'In progress',
    [TodoStatus.Completed]: 'Completed',
};

const onDraggableChange =(payload: any) => {
    if (payload?.added?.element) {
    // update todo status
    updateTodo(payload?.added?.element, props.status);
  }
}
</script>

<template>
  <div class="group-wrapper">
   <h3>{{ groupLabel[props.status] }}</h3>

    <draggable
      class="draggable"
      :list="todoList"
      group="todos"
      itemKey="id"
      @change="onDraggableChange"
    >

        <template #item="{ element: todo }">
          <li>
            {{ todo.title}}
            <span class="delete-icon" @click="deleteTodo(todo)">x</span>
            <div>
              <span class="todo-description">{{todo.description}}</span>
            </div>
          </li>
        </template>
    </draggable>

    <CreateTodo :status="props.status"/>
  </div>
</template>

<style scoped>
.group-wrapper {
  flex: 1;
  padding: 20px;
  background-color: #0c6f7e;
  width: 300px;
}

.group-wrapper li {
  list-style-type: none;
  background-color: #c4e9ed;
  color: #0c6f7e;
  padding: 2px 5px;
  cursor: grab;
  margin-bottom: 10px;
}


.draggable {
  min-height: 200px;
}
.delete-icon {
  float: right;
  cursor: pointer;
}

.todo-description {
  font-size: 12px;
}

</style>
