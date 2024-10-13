<template>
    <h1 class="typing-heading">
      {{ displayedText }}<span class="cursor">|</span>
    </h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        text: "Welcome to Lingomate", 
        displayedText: "", 
        index: 0, // 
        typingSpeed: 100, 
        clearingSpeed: 100, 
        isClearing: false 
      };
    },
    mounted() {
      this.type(); 
    },
    methods: {
      type() {
        if (!this.isClearing) {
          if (this.index < this.text.length) {
            this.displayedText += this.text.charAt(this.index); 
            this.index++;
            setTimeout(this.type, this.typingSpeed); 
          } else {
            this.isClearing = true; 
            setTimeout(this.clearText, 4000); 
          }
        }
      },
      clearText() {
        if (this.index > 0) {
          this.displayedText = this.displayedText.slice(0, -1); 
          this.index--; 
          setTimeout(this.clearText, this.clearingSpeed); 
        } else {
          this.isClearing = false; 
          this.index = 0; 
          this.displayedText = ""; 
          this.type(); 
        }
      }
    }
  };
  </script>
  <style scoped>
 
  @keyframes blink {
    50% { background-color: transparent; } 
  }
  </style>
  