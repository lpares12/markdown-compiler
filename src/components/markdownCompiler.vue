<template>
  <div class="container" v-html="sanitize(parse(message))"></div>
</template>
<script>
const sanitizeHtml = require('sanitize-html');

const sanitizeOptions = {
  allowedTags: ['div', 'br', 'span', 'b', 's'],
  allowedAttributes: {
    'div': [ 'class' ],
    'span': [ 'class' ]
  }
};

export default{
  name: 'markdownCompiler',
  props: {
    message: {
      type: String,
      required: true,
    },
  },
  methods: {
    parse(text){
      if(!text) return "";
      const toHtml = text
            //Do not allow line jumps when there is a <caution>, <example> or <tip> tag
            //since they render into div which will be already with some margins
            .replace(/(\\n)*<(\/?example|\/?caution|\/?tip)>(\\n)*/gm, "<$2>")
            // Replace ** something ** with <b>something</b>
            .replace(/\*\*(.*?)\*\*/gim, "<b>$1</b>")
            //Replace <color>something</color> for <span class="color">something</span>
            .replace(/<color>(.*?)<\/color>/gims, "<span class='color'>$1</span>")
            //Replace multiple \n for one <br>
            .replace(/(\\n)+/gm, "<br>")
            //Replace <caution>something</caution> for <div class="caution">something</div>
            .replace(/<caution>(.*?)<\/caution>/gims, "<div class='caution'>$1</div>")
            //Replace <example>something</example> for <div class="example">something</div>
            .replace(/<example>(.*?)<\/example>/gims, "<div class='example'>$1</div>")
            //Replace <example>something</example> for <div class="example">something</div>
            .replace(/<tip>(.*?)<\/tip>/gims, "<div class='tip'>$1</div>")
            //Replace ~~something~~ for <s>something</s>
            .replace(/~~(.*?)~~/gim, "<s>$1</s>")
            //Replace [ab]{cda} for <span class="kanji">ab<span>cd</span></span>
            .replace(/\[([^\][]+)\]\{([^{}]+)\}/gim, "<span class='kanji'>$1<span>$2</span></span>")
      return toHtml.trim();
    },
    sanitize(text){
      return sanitizeHtml(text, sanitizeOptions);
    }
  }
}
</script>

<style>
.container {
  padding: 10px;
  border: 1px solid;
  border-color: black;
}

.color {
  color: green;
}

.caution {
  padding: 10px;
  margin: 10px;
  border: 1px solid;
  border-color: red;
}

.caution > .example {
  padding: 10px;
  margin: 10px;
  border: 1px dashed;
  border-color: blue;
}

.example {
  padding: 10px;
  margin: 10px;
  border: 1px solid;
  border-color: blue;
}

.tip {
  padding: 10px;
  margin: 10px;
  border: 1px solid;
  border-color: green;
}

/* Display kanji with furigana on top */
.kanji {
  display: inline-flex;
  flex-direction: column-reverse;
  text-align: center;
}

/* Furigana */
.kanji > span {
  font-size: 10px;
}
</style>
