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
  mounted: function() {
      console.log('character:',this.character)
  },
  updated: function() {
      console.log('character:',this.character)
    if (this.character === "F") {
        this.display = "🚩";
        this.className = "flag-square";
    } else if (this.character === "*") {
        this.display = "💣";
        this.className = "bomb-square";
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
    } else if (this.character === '_') {
        this.display = "_"
        this.className = "blank-square"
    }
  }
};
</script>

<style>
</style>
