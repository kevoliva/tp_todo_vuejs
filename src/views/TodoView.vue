<template>
  <div class="todo">
    <h1>Il y a {{todos.length }} todo,</h1>
    <h2>Dont {{ todos.filter((todo) => todo.isSelected == true).length }}
        actuellement sélectionné(s) !</h2>
    <img class="logo-todo" alt="Vue logo" src="../assets/todo_logo.png">
    <h4 v-if="isInvalid">Veuillez renseigner tous les champs, en respectant les validations !</h4>
    <div class="row mt-6 add-user">
        <form @submit.prevent="addTodo()">
            <label for="inputName">
                Nom
                <input v-model="name" class="form-input" id="inputName" type="text">
            </label>
            <label for="inputHour">
                Nombre d'heures
                <input v-model="hour" class="form-input" id="inputHour" type="number" min="1">
            </label>
            <label for="selectUser">
                Utilisateur
                <select v-model="user" class="form-input" id="selectUser">
                    <option value="Quentin">Quentin</option>
                    <option value="Théo">Théo</option>
                    <option value="Paul">Paul</option>
                    <option value="Hugo">Hugo</option>
                </select>
            </label>
            <button type="submit" class="btn-add">Ajouter</button>
        </form>
    </div>
    <div class="row mt-6 list">
        <div class="one-line-todo mt-3"
        v-for="(todo, index) in todos" v-bind:key="index" :class="todos[index].bg">
            <div class="element name"
            @click="select(index)" @keyPress="select(index)">{{ todo.name }}</div>
            <div class="element hour">{{ todo.hour }}</div>
            <div class="element user">{{ todo.user }}</div>
            <div class="element status">{{ todo.status }}</div>
            <div>
                <button type="button" class="btn-action btn-edit">
                    <font-awesome-icon icon="fa-solid fa-pen" />
                </button>
                <button type="button" @click="todos.splice(index, 1)" class="btn-action btn-delete">
                    <font-awesome-icon icon="fa-solid fa-trash" />
                </button>
            </div>
        </div>
        <button v-if="todos.length > 0" @click="todos=[]" class="btn-delete-all mt-3" type="button">
            Supprimer toutes les tâches
        </button>
        <button v-if="todos.filter((todo) => todo.isSelected == true).length > 0"
            @click="deleteSelected()" class="btn-delete-all mt-3" type="button">
            Supprimer les éléments sélectionnés
        </button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import TodoComponent from '@/components/TodoComponent.vue';

export default {
  name: 'TodoView',
  components: {
    // TodoComponent,
  },
  data() {
    return {
      todos: [],
      name: '',
      hour: '',
      user: '',
      status: '',
      bg: '',
      isSelected: false,
      isInvalid: false,
    };
  },
  methods: {
    addTodo() {
      if (this.checkInputs()) {
        this.todos.push({
          name: this.name,
          hour: this.hour,
          user: this.user,
          status: '🔴 A venir',
          bg: '',
        });
        this.name = '';
        this.hour = '';
        this.user = '';
        this.bg = '';
      }
    },
    checkInputs() {
      if (this.name === '' || this.hour === '' || this.user === '' || this.hour <= 0) {
        this.isInvalid = true;
        return false;
      }
      this.isInvalid = false;
      return true;
    },
    select(index) {
      if (this.todos[index].isSelected) {
        this.todos[index].isSelected = false;
        this.todos[index].bg = '';
      } else {
        this.todos[index].isSelected = true;
        this.todos[index].bg = 'item-selected';
      }
    },
    deleteSelected() {
      this.todos = this.todos.filter((todo) => todo.isSelected !== true);
    },
  },
};
</script>

<style scoped>
    .logo-todo {
        width: 120px;
        height: 120px;
    }

    .row {
        display: block;
    }

    .mt-3 {
        margin-top: 30px;
    }

    .mt-6 {
        margin-top: 60px;
    }

    .form-input {
        font-size: 16px;
        padding: 12px 24px;
        border-radius: 10px;
        background-color: white;
        margin-right: 30px;
        appearance: none;
        outline: none;
    }

    input, select {
        margin-left: 10px;
    }

    .btn-add {
        border: none;
        border-radius: 20px;
        font-size: 16px;
        padding: 12px 24px;
        background: #1abc9c;
        color: white;
        font-weight: 700;
        cursor: pointer;
        transition: all .6s ease-in-out;
    }

    .btn-add:hover {
        background: #16a085;
    }

    .one-line-todo {
        display: flex;
        justify-content: center;
        align-items: center;
        border: solid 1px #333;
        border-radius: 28px;
        width: 80%;
        margin-left: auto;
        margin-right: auto;
        padding: 20px 6px;
    }

    .name {
        cursor: pointer;
    }

    .item-selected {
        background: #bdc3c7;
    }

    .element {
        width: 20%;
        text-align: center;
        font-size: 18px;
        font-weight: 700;
    }

    h4 {
        font-size: 30px;
        color: #c0392b;
    }

    .btn-action {
        margin-left: 5px;
        margin-right: 5px;
        border: none;
        border-radius: 50%;
        padding: 12px;
        cursor: pointer;
        transition: all .3s ease-in-out;
    }

    .btn-edit {
        color: #f39c12;
    }

    .btn-delete {
        color: #c0392b;
    }

    .btn-edit:hover {
        background-color: #f1c40f;
        color: white;
    }

    .btn-delete:hover {
        background-color: #e74c3c;
        color: white;
    }

    .btn-delete-all {
        display: block;
        margin-right: auto;
        margin-left: auto;
        border: none;
        border-radius: 20px;
        padding: 12px 24px;
        cursor: pointer;
        transition: all .3s ease-in-out;
        background-color: #c0392b;
        color: white;
        font-size: 20px;
    }

    .btn-delete-all:hover {
        background-color: white;
        color: #c0392b;
        border: solid 3px #c0392b;
    }
</style>
