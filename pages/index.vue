<template>
  <v-form>
    <v-container>
      <v-card flat outlined>
        <v-card-title> Let's Todo app! </v-card-title>
      </v-card>
      <v-card>
        <v-text-field
          v-model="todo"
          label="Input your todo"
          filled
        ></v-text-field>
        <v-btn @click="addTodo(todo)">ADD</v-btn>
        <v-btn @click="clean()">clean</v-btn>
      </v-card>

      <v-card
        v-for="(value, key) in todos"
        :key="key"
        elevation="2"
        width="400"
        color="blue"
      >
        <v-text-field v-model="todos[key].todo"></v-text-field>

        <v-card-actions>
          <v-btn depressed @click="update(key)"> update </v-btn>
          <v-btn depressed @click="deleteTodo(key)"> delete </v-btn>
          <v-checkbox v-model="todos[key].state"> </v-checkbox>
        </v-card-actions>
      </v-card>
    </v-container>
  </v-form>
</template>

<script>
export default {
  data: () => ({
    todo: '',
    updated: '',
    todos: [],
  }),
  created() {
    this.readTodo()
  },
  methods: {
    clean() {
      this.todo = ''
    },
    async addTodo(todo) {
      this.todos.unshift({ todo })
      const url = 'https://c1p4r1tav6.microcms.io/api/v1/todo/'
      const header = {
        headers: {
          'X-MICROCMS-API-KEY': 'b2fd1a5004024670b6e584f27487d74b4059',
        },
      }
      const data = { todo, state: false }
      await this.$axios.$post(url, data, header)
      this.readTodo()
      // this.todos.todo.unshift(TODO)
    },
    deleteTodo(key) {
      const url =
        'https://c1p4r1tav6.microcms.io/api/v1/todo/' + this.todos[key].id
      const header = {
        headers: {
          'X-MICROCMS-API-KEY': 'b2fd1a5004024670b6e584f27487d74b4059',
        },
      }
      this.todos.splice(key, 1)
      this.$axios.$delete(url, header)
    },

    async update(key) {
      // this.todos.push(key + 1)

      // this.todos.unshift({ todo: this.todos[key].todo })
      const url =
        'https://c1p4r1tav6.microcms.io/api/v1/todo/' + this.todos[key].id
      const header = {
        headers: {
          'X-MICROCMS-API-KEY': 'b2fd1a5004024670b6e584f27487d74b4059',
        },
      }
      const data = { todo: this.todos[key].todo, state: this.todos[key].state }
      await this.$axios.$patch(url, data, header)
      this.readTodo()
    },
    async readTodo() {
      const url = 'https://c1p4r1tav6.microcms.io/api/v1/todo'
      const header = {
        headers: {
          'X-MICROCMS-API-KEY': 'b2fd1a5004024670b6e584f27487d74b4059',
        },
      }
      const response = await this.$axios.$get(url, header)
      this.todos = response.contents
    },
  },
}
</script>
