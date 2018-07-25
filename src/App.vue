<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="(item,index) in items"
        :key="index"
        :class="{finished: item.isFinished}"
        v-on:click="toggleFinish(item)">
        {{ item.label }}
      </li>
    </ul>
    <p>child tells me: {{childWords}}</p>
    <component-a
      msgfromfather='you die'
      v-on:child-tell-me-somthing='listenToMyBoy'></component-a>
    <router-view/>
  </div>
</template>

<script>
import Store from './store'
import ComponentA from './components/componentA'
export default {
  name: 'App',
  data: function () {
    return {
      title: 'this is a todo list',
      items: Store.fetch(),
      newItem: '',
      childWords: ''
    }
  },
  components: {
    ComponentA
  },
  watch: {
    items: {
      handler: function (items) {
        Store.save(items)
      },
      deep: true
    }
  },
  methods: {
    toggleFinish: function(item) {
      item.isFinished = !item.isFinished
    },
    addNew() {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
      Store.save()
    },
    listenToMyBoy (msg) {
      this.childWords = msg
    }
  }
}
</script>

<style>
.finished {
  text-decoration: underline;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
