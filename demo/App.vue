<template>
  <AccDropdown :items="[{value: 1, content: 'test1'}, {value: 2, content: 'item2'}]" v-model="value"/>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import AccDropdown from './components/AccDropdown.vue'
import { ref } from 'vue'

export default defineComponent({
  name: 'App',
  components: {
    HelloWorld,
    AccDropdown
  },
  setup() {
    return {
      value: ref(1),
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
