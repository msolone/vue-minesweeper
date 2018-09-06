<template>
    <span :class="className">{{display}}</span>
</template>

<script>
export default {
  props: {
    character: String
  },
  data: function() {
    return {
      display: " ",
      className: ""
    };
  },
  updated: function() {
      console.log(this.character)
    if (this.character === "F") {
        this.display = "🚩";
    } else if (this.character === "*") {
        this.display = "💣";
    } else if (this.character === "1") {
        this.display = "1";
        this.className = "blue-num";
    } else if (this.character === '2') {
        this.display = '2';
        this.className = "green-num"
    } else if (this.character === '3') {
        this.display = '3';
        this.className = "red-num"
    } else if (this.character === '4') {
        this.display = '4';
        this.className = "purple-num"
    } else if (this.character === '5') {
        this.display = '5';
        this.className = "black-num"
    } else if (this.character === '6') {
        this.display = '6';
        this.className = "maroon-num"
    } else if (this.character === '7') {
        this.display = '7';
        this.className = "gray-num"
    } else if (this.character === '8') {
        this.display = '8';
        this.className = "turquoise-num"
    }
  }
};
</script>

<style>
</style>
