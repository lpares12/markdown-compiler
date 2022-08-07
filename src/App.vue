<template>
<textarea v-model="input"></textarea>
<br>
<br>
<MarkdownCompiler :message="input" :key="input"/>
<br>
<br>
<button v-on:click="copyToClipboard">Copy markdown to clipboard</button>
</template>

<script>
import MarkdownCompiler from '@/components/markdownCompiler.vue'

export default {
  name: 'App',
  components: {
    MarkdownCompiler
  },
  data(){
    return {
      input: "",
    }
  },
  methods: {
    copyToClipboard(){
      navigator.clipboard.writeText(this.input);
    }
  }
}
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
