<template>
  <link rel="stylesheet" type="text/css" href="http://fonts.googleapis.com/css?family=Ubuntu:regular,bold&subset=Latin">
  <div id="app">
    <!-- DARK MODE LIGHT MODE -->
    <header> 
      <button class="GITH DARK">
        <a class="Git" href="https://github.com/VineetMittal007/LingoMate" target="_blank">
          GitHub 
    </a>
    <img src="https://upload.wikimedia.org/wikipedia/commons/9/91/Octicons-mark-github.svg" alt="GitHub Logo" class="github-logo ImageGit" />
    </button>
    <button class="DARK" @click="toggleDarkMode">
      <i :class="darkMode ? 'fas fa-sun' : 'fas fa-moon'"></i>
      {{ darkMode ? 'Switch to Light Mode' : 'Switch to Dark Mode' }} 
    </button>
    </header>
    <header class="lingo">
        <TypingEffect/>
    </header>
    <TranslateInput @translate="handleTranslate" />
    <TranslationOutput :translation="translation" />
    <HistoryList :history="history" /> 
  </div>
  <footer class="app-footer">
    Made with <span class="heart">❤️</span> by <a href="https://github.com/VineetMittal007" target="_blank">VINEET_MITTAL</a>
  </footer>
</template>

<script>

import TranslateInput from './components/TranslateInput.vue';
import TypingEffect from './components/TypingEffect.vue';

export default {
  data() {
    return {
      darkMode: false,
      translation: '',
      history: [],
    };
  },
  components: {
    TypingEffect,
    TranslateInput,
  },
  methods: {
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
      document.body.classList.toggle('dark-mode', this.darkMode);
    },
  },
};
</script>

<style scoped>
.GITH{
  color: black;
  
}
a {
  text-decoration: none;
}
.Git{
  padding-right: 5px;
  color: rgb(0, 0, 0);
}
.github-logo {
  width: 15px;
  height: 15px;
}
.github-link a:hover .github-logo {
  filter: brightness(0.8); 
}
</style>