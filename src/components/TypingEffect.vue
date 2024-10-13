<template>
    <h1 class="typing-heading">
      {{ displayedText }}<span class="cursor">|</span>
    </h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        text: "Welcome to Lingomate", // The text to type
        displayedText: "", // The text currently displayed
        index: 0, // The current character index
        typingSpeed: 100, // Speed of typing in milliseconds
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
          this.displayedText = this.displayedText.slice(0, -1); // Remove last character
          this.index--; // Decrement index
          setTimeout(this.clearText, this.clearingSpeed); // Call clearText again after delay
        } else {
          // Finished clearing, reset everything
          this.isClearing = false; // Reset the flag
          this.index = 0; // Reset index
          this.displayedText = ""; // Clear displayed text
          this.type(); // Restart typing
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
  