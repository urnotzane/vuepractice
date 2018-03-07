<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input type="text" v-model="newItem" v-on:keyup.enter="addNew"/>
    <ul>
      <li v-for="item in items" v-bind:class="{ deco: item.isfinished }" v-on:click="toggleFinished(item)">
        {{item.label}}
      </li>
    </ul>
  </div>
</template>

<script>
import Store from "../store"

export default {
  data: function() {
    return {
      title: "this a vue app",
      items: Store.fetch(),
      newItem: ""
    }
  },
  methods: {
    //切换class
    toggleFinished: function(item) {
      item.isfinished = !item.isfinished;
    },
    //添加新item
    addNew: function() {
      var that = this;
      that.items = that.items || []
      that.items.push({label: this.newItem,isfinished: false});
      this.newItem = "";
    }
  },
  watch: {
    items: {
      handler: function (items) {
        Store.save(items)
      },
      deep: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  color: #42b983;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
.deco {
  text-decoration: line-through;
}
</style>
