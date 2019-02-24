<template>
  <div class="tag-container cf">
    <div v-for="(tag, index) in tags" 
      :key="index"
      class="tag-label">
      <span class="tag-label-text">{{tag}}</span>
      <a href="#" class="tag-remove" @click.stop.prevent="remove(index)">
        <svg xmlns="http://www.w3.org/2000/svg" 
          xmlns:xlink="http://www.w3.org/1999/xlink" 
          viewBox="0 0 50 50" version="1.1" width="15px" height="15px">
          <g id="surface1">
            <path style=" " d="M 7.71875 6.28125 L 6.28125 7.71875 L 23.5625 25 L 6.28125 42.28125 L 7.71875 43.71875 L 25 26.4375 L 42.28125 43.71875 L 43.71875 42.28125 L 26.4375 25 L 43.71875 7.71875 L 42.28125 6.28125 L 25 23.5625 Z "/>
          </g>
        </svg>
      </a>
    </div>
    <div class="editor">
      <input class="input" 
        ref="input" 
        type="text"
        placeholder="ここに入力"
        @keyup.enter="enter($event.target)"
        @keypress="canEnter = true"/>
    </div>
  </div>
</template>

<script>
export default {

  props: {
    value: {
      type: String,
      required: true,
    },
  },

  data() {
    return { 
      tags: [],
      canEnter: false,
    }
  },

  methods: {

    propToData() {
      console.log("propToData")
      this.tags.length = 0
      this.value.split(/,/).forEach(val => this.tags.push(val))
    },

    enter(target) {
      //日本語の確定で EnterキーのKeyupが発生するのを抑止
      if (!this.canEnter) return

      if (typeof target.value === "string" && target.value.trim() != "") {
        this.tags.push(target.value.trim().replace(/,/, ""))
        target.value = ""
      }
      this.canEnter = false
    },

    remove(index) {
      this.tags.splice(index, 1);
    }
  },

  watch: {
    value: function(newVal, oldVal) {
      if (this.tags.join(",") != newVal) {
        this.propToData()
      }
    }
  },

  mounted() {
    this.propToData()
  },

  updated() {
    this.$emit('input', this.tags.join(","))
  }
}
</script>

<style scoped>

.tag-container {
  padding: 5px 5px 0 5px;
  border: 1px solid #ccc;
  margin: 20px;
  font-size: 15px;
}
.tag-container .tag-label,
.tag-container .editor {
  float: left;
}
.tag-container .tag-label {
  background: #EBF4FB;
  padding: 0 2.1em 0 12px;
  margin: 0 5px 5px 0;
  height: 2em;
  border-radius: 1em;
  box-sizing: border-box;
  position: relative;
}
.tag-label-text {
  display: inline-block;
  height: 100%;
  margin: 0;
  padding: .5em 0 0 0;
  line-height: 1;
}
.tag-container a.tag-remove {
  height: 100%;
  width: 2em;
  display: block;
  position: absolute;
  border-radius: 2em;
  right: 0;
  top: 0;
  margin: 0;
  padding: 0;
}
.tag-container a.tag-remove:hover {
  background: #bbd8f1;
}
.tag-container .tag-label svg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
.tag-container .editor .input {
  border: 0px;
  outline: 0;
  background: transparent;
  padding: 3px 4px;
  min-width: 5em;
  width: auto;
}
.cf::after {
  content: "";
  clear: both;
  display: table;
}
</style>
