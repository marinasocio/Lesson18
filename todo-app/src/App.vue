<template>
  <div id="app">
    <!-- <div class="container"> -->
      <div class="row justify-content-md-center">
        <div class="col col-md-6">
          <b-card
            title="Todo app"
            img-src="https://picsum.photos/id/119/3264/2176"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 100%"
            class="mb-2">
            <div class="card">
            <form class="card-body" @submit.prevent="add(todo)">
              <div class="input-group">
                <input type="text" class="form-control" placeholder="Add todo" v-model="todo">
                <span class="input-group-btn">
                  <button class="btn btn-secondary" type="submit" :disabled="!todoIsValid">Add</button>
                </span>
              </div>
            </form>

              <ul class="list-group list-group-flush">
                <li class="list-group-item" v-for="todo in todos" v-bind:key="todo.id">
                  <div class="d-flex" v-if="!todo.isEdited">
                    <div class="p-2">
                      <div class="form-check">
                        <input type="checkbox" class="form-check-input" v-model="todo.finished" />
                        <label class="form-check-label">
                          <span :class="{'todo-finished': todo.finished}">{{ todo.content }}</span>
                        </label>
                      </div>
                    </div>
                    <div class="ml-auto p-2">
                      <button
                        class="btn btn-sm btn-dark"
                        @click="edit(todo)"
                        v-show="!isBeingEdited">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </div>
                  <form v-else class="m-0" @submit.prevent="update(todo)">
                    <div class="input-group">
                      <input
                        type="text"
                        class="form-control form-control-sm"
                        placeholder="Edit"
                        v-model="editTodo"/>
                      <div class="btn-group" role="group" aria-label="Basic example">
                        <button
                          class="btn btn-outline-warning"
                          type="submit"
                          :disabled="!editTodoIsValid">
                          <i class="fa fa-save"></i>
                        </button>
                        <button class="btn btn-outline-danger" @click="remove(todo)">
                          <i class="fa fa-trash"></i>
                        </button>
                      </div>
                    </div>
                  </form>
                </li>
              </ul>
            </div>
          </b-card>
        </div>
      </div>
    </div>
  <!-- </div> -->
</template>

<script>
export default {
  name: "todo-app",
  data() {
    return {
      todo: null,
      todos: [],
      editTodo: null,
    };
  },
  methods: {
    add(todo) {
      if (!this.todoIsValid) {
        return;
      }
      this.todos.push({ content: todo, finished: false, isEdited: false });
      this.todo = null;
    },
    remove(todo) {
      this.todos = this.todos.filter(item => {
        return item != todo;
      });
    },
    edit(todo) {
      this.editTodo = todo.content;
      todo.isEdited = true;
    },
    update(todo) {
      if (!this.editTodoIsValid) {
        return;
      }
      todo.content = this.editTodo;
      todo.isEdited = false;
    }
  },
  computed: {
    todoIsValid() {
      return !!this.todo;
    },
    editTodoIsValid() {
      return !!this.editTodo;
    },
    isBeingEdited() {
      return this.todos.filter(todo => todo.isEdited).length > 0;
    }
  }
};
</script>

<style>
html,
body {
  background: #fafafa;
}
#app {
  font-family: "Montserrat", sans-serif;

  text-align: center;
  color: #000;
  margin-top: -90px;
  background: #fafafa;
}
.todo-finished {
    text-decoration: line-through;
    color: grey;
}
</style>
