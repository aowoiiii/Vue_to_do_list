<template>
  <div id="app">
    <h2>{{title}}</h2>
    <input v-model="newItem" v-on:keyup.enter="addNew"/>
    <ol>
      <li v-for="item in items" class="item">
        <span :class="{finished:item.isFinished}" @click="toggleFinish(item)">{{item.label}}</span>
        <span v-if="item.isFinished" class="finished-item">finished</span>
        <button @click="delItem(item)" class="del">Delete</button>
      </li>
    </ol>
  </div>
</template>

<script>
  import Store from './store'

  export default {
    data() {
      return {
        title: 'To Do List - vue.js',
        items: Store.fetch(),
        newItem: ''
      }
    },
    watch: {
      items: {
        handler(item) {
          Store.save(item)
        },
        deep: true
      }
    },
    methods: {
      toggleFinish(item) {
        console.log(item.isFinished = !item.isFinished);
      },
      addNew() {
        var flag = true;
        console.log(this.items.length)
        for (var i = 0; i < this.items.length; i++) {
          if (this.newItem == this.items[i].label) {
            alert('已有相同')
            flag = false
            break
            //console.log(this.items[i].label);
          }
        }
        if (flag) {
          this.items.push({
            label: this.newItem,
            isFinished: false,
          });
          this.newItem = ''
        }
      },
      delItem(item) {
        //console.log(item)
        for (var i = 0; i < this.items.length; i++) {
          if (item == this.items[i]) this.items.splice(i, 1);
        }
      }
    }
  }
</script>

<style>

  li {
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 10px;
  }

  .finished {
    text-decoration: line-through;
  }

  .finished-item {
    font-size: 12px;
    background: deeppink;
    color: white;
    padding: 2px 5px 2px 5px;
  }

  .del {
    font-size: 15px;
    float: right;
    color: white;
    line-height: 20px;
    background: red;
    border: 0;
    height: 20px;
  }

  .del:hover {
    background: deeppink;
  }

  input {
    width: 600px;
    height: 30px;
    font-size: 20px;
    line-height: 30px;
    border: 1px solid;
    padding-left: 10px;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
    width: 600px;
    margin: auto;
  }
</style>
