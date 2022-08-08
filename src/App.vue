<template>
<textarea v-model="input" v-on:keyup="processInput()"></textarea>
<br>
<br>
<MarkdownCompiler :message="process(input)" :key="input"/>
<br>
<br>
<button v-on:click="copyToClipboard(process(input))">Copy markdown to clipboard</button>
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
    process(text){
      //Transform \r\n or \n to \\n. This is due to MongoDB not accepting
      //the \r\n and \n characters in strings. And we will need to save to DB
      return text.replace(/[\r\n]+/gm, "\\n");
    },
    copyToClipboard(text){
      navigator.clipboard.writeText(text);
    },
    processInput(){
      //In case we copy-paste from a compiled markdown, we need to perform the reverse
      //replace, for the text input to show the string easily.
      this.input = this.input.replace(/(\\n)+/gm, "\n");
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
