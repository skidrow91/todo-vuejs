<template>
  <div class="wrapper">
    <h3 class="title">{{title}}</h3>
    <div class="todo-block">
      <div class="container">
        <div class="text-fields">
          <!-- <input type="text" class="field" v-on:keyup.enter="add($event.target.value)" /> -->
          <input type="text" class="field" v-model="newTodo" v-on:keyup.enter="add()" />
        </div>
        <div class="lists">
          <ul class="list-todos" v-if="activeItems.length">
            <li v-for="item in activeItems" :key="item.id">
              <div class="todo-item">
                <input type="checkbox" v-bind:id="'item-'+item.id" v-model="item.isCompleted">
                <label class="item-name" v-bind:for="'item-'+item.id">{{item.name}}</label>
                <a href="#" v-on:click="del(item.id)" class="item-remove"><font-awesome-icon icon="trash-alt" /></a>
              </div>
            </li>
          </ul>
          <div class="no-items" v-if="!activeItems.length">
            <h3>No Items</h3>
          </div>
        </div>
        <div class="footer">
          <div class="total-items">{{countedItems()}} item(s) left</div>
          <div class="statuses">
            <ul class="list-statuses">
              <li v-for="(item, index) in statuses" :key="index" v-bind:class="{active: (currentStatus == item)}">
                <a href="#" v-on:click="show(item)">{{item}}</a>
              </li>
            </ul>
          </div>
          <div class="clear-completed">
            <button type="button" v-on:click="delCompleted()">Delete Completed</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  let uid = 0;
  export default {
    name: 'Todo',
    methods: {
      add: function () {
        if (this.newTodo.length == 0) return false;
        let obj = {
          id: ++uid,
          name: this.newTodo,
          status: 'active'
        };
        this.todos.push(obj);
        this.newTodo = '';
      },
      countedItems: function () {
        let count = 0;
        for (let i=0; i<this.todos.length; i++) {
          if (!this.todos[i].isCompleted) {
            count++;
          }
        }
        return count;
      },
      delCompleted: function () {
        let index = 0;
        while (index < this.todos.length) {
          if (this.todos[index].isCompleted) {
            this.todos.splice(index, 1);
          }
          else {
            index++;
          }
        }
      },
      del: function (id) {
        for (let i=0; i<this.todos.length; i++) {
          if (this.todos[i].id == id) {
            this.todos.splice(i, 1);
          }
        }
      },
      show: function (status) {
        this.currentStatus = status;
      }
    },
    data() {
      return {
        title: 'todos',
        currentStatus: 'all',
        newTodo: '',
        statuses: ['all', 'active', 'completed'],
        todos: [
          {
            id: ++uid,
            name: 'test1',
            isCompleted: false
          },
          {
            id: ++uid,
            name: 'test2',
            isCompleted: false
          },
          {
            id: ++uid,
            name: 'test3',
            isCompleted: false
          },
          {
            id: ++uid,
            name: 'test4',
            isCompleted: false
          }
        ]
      }
    },
    computed: {
      activeItems: function () {
        let self = this;
        return this.todos.filter(function(item){
          if (self.currentStatus == 'completed') {
            return item.isCompleted;
          }
          else {
            if (self.currentStatus == 'active') {
              return !item.isCompleted;
            }
          }
          return true;
        });
      }
    }
  }
</script>