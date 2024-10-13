<template>

  <!-- MAINPROGRAM -->
  <div class="CONTENT">
   <div class="container">
    <!-- INPUT TEXT SIDE  -->
      <div class="card input-wrapper">
        <!-- DROPDOWNBUTTON -->
        <div class="from">
          <span class="heading">From :</span>
          <div class="dropdown-container">
          <div class="dropdown-toggle" ref="inputDropdown" @click="toggleDropdown('input')">
          <span class="selected">{{ inputLanguage.name }} ({{ inputLanguage.native }})</span>
          <i class="fas fa-chevron-down" style="color: black;"></i> 
          </div>

          <!-- Dropdown menu  -->
          <ul v-if="inputDropdownActive" class="dropdown-menu">
          <li v-for="language in languages" :key="language.code"
          :class="{'active': inputLanguage.code === language.code}" 
          @click.stop="selectInputLanguage(language)">
          {{ language.name }} ({{ language.native }})
          </li>
          </ul>
          <!-- Dropdown menu END!! -->
        </div>
      </div>
      <!-- DROPDOWNBUTTON END -->

      <!-- Input Text Area -->
        <div class="text-area">
          <textarea
            id="input-text"
            v-model="inputText"
            @input="limitInput"
            cols="30"
            rows="10"
            placeholder="Enter your text here"
          ></textarea>
        </div>
      <!-- Input Text Area END-->
      
      <!-- CHARACTERCOUNT -->
        <div class="chars pareshan"><span>{{ inputText.length }}</span> / 5000</div>
      <!-- CHARACTERCOUNT END-->

      <!-- File Upload -->
        <div class="card-bottom">
          <p>Or choose your document!</p>
            <label for="upload-document">
              <span class="down" id="upload-title">{{ uploadedFileName || 'Choose File' }}</span>
                <ion-icon class="Downicon" name="cloud-upload-outline"></ion-icon>
              <input type="file" id="upload-document" @change="handleFileUpload" hidden />
            </label>
        </div>
      <!-- File UploadEnd -->

      </div>
      <!-- INPUT TEXT SIDE END -->





      <!-- Swap Button -->
        <div class="center">
          <div class="swap-position" @click="swapLanguages">
            <i class="fas fa-exchange-alt"></i>
          </div>
        </div>
      <!-- Swap Button END-->





      <!-- Output Language Dropdown -->
      <div class="card output-wrapper">
        <!-- DROPDOWN -->
        <div class="to">
          <span class="heading">To :</span>
            <div class="dropdown-container">
              <div class="dropdown-toggle" ref="outputDropdown" @click="toggleDropdown('output')">
              <span class="selected">{{ outputLanguage.name }} ({{ outputLanguage.native }})</span>
              <i class="fas fa-chevron-down" style="color: black;"></i> 
              </div>
              <ul v-if="outputDropdownActive" class="dropdown-menu">
                <li v-for="language in languages" :key="language.code" 
                  :class="{'active': outputLanguage.code === language.code}" 
                  @click="selectOutputLanguage(language)">
                {{ language.name }} ({{ language.native }})
                </li>
              </ul>
            </div>
        </div>
        <!-- DROPDOWN END -->


        <!-- Output Text Area -->
        <textarea
          id="output-text"
          v-model="outputText"
          cols="30"
          rows="10"
          placeholder="Translated text will appear here"
          disabled
        ></textarea>
        <!-- Output Text AreaEnd -->
         <!-- TTS -->
         <!-- <button @click="speakText" class="sound-button"> -->
              <!-- <i class="g fas fa-volume-up"></i>  Font Awesome sound icon -->
          <!-- </button> -->
          <!-- TTS END-->
        <button @click="translate" class="translate-button transbut">Translate</button>

        <!-- Download Button -->
        <div class="card-bottom">
          <p>Download as a document!</p>
          <button class="down" id="download-btn" @click="downloadTranslatedFile">
            <span class="down">Download</span>
            <ion-icon class="Downicon" name="cloud-download-outline"></ion-icon>
          </button>
        </div>
        <!-- Download Button End-->

      </div>
      <!-- Output Language Dropdown End-->
    </div>
  </div>
  <!-- MAINPROGRAMEND -->



  <!-- HISTORY -->
    <div class="history-container">
      <h3 class="history-title">Translation History</h3>
      <ul class="history-list">
        <transition-group name="slide" tag="ul">
          <li class="history-item" v-for="(entry, index) in history" :key="index" @click="toggleDetails(index)">
            <div class="history-summary">
              <strong class="history-lang">{{ entry.inputLanguage }} → {{ entry.outputLanguage }}</strong>
              <span class="timestamp">{{ entry.timestamp }}</span>
              <button class="delete-button" @click.stop="deleteHistory(index)">
                <i class="fas fa-trash-alt"></i>
              </button>
            </div>
            <transition name="fade">
              <div v-if="expandedIndex === index" class="history-details">
                <div class="history-text">
                  <strong>Input:</strong> {{ entry.inputText }}
                </div>
                <div class="history-text">
                  <strong>Output:</strong> {{ entry.outputText }}
                </div>
              </div>
            </transition>
          </li>
        </transition-group>
      </ul>
      <!-- <div v-if="snackbarMessage" class="snackbar">{{ snackbarMessage }}</div> THIS LINE DOES NOT WORK FOR SOME REASON -->
    </div>
  <!-- HISTORYEND -->

</template>




<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faChevronDown } from '@fortawesome/free-solid-svg-icons';
library.add(faChevronDown);

export default {
  data() {
    return {
      expandedIndex: null,
      snackbarMessage: '',
      inputText: 'This is a sample Input. Click on the transalate button. Yoou can select multiple languages from the dropdown menu. There is a History tab that stores all your last 10 Inputs. You can also click on the Speaker button for Text to speech of the translated output.',
      outputText: '',
      inputLanguage: { name: 'English', native: 'English', code: 'en' },
      outputLanguage: { name: 'Hindi', native: 'हिन्दी', code: 'hi' },
      inputDropdownActive: false,
      outputDropdownActive: false,
      uploadedFileName: '',
      isDarkMode: false,
      history: [],
      languages: [ //language options
  { name: "Auto Detect", native: "Detect", code: "auto" },
  { name: "Afrikaans", native: "Afrikaans", code: "af" },
  { name: "Albanian", native: "Shqip", code: "sq" },
  { name: "Arabic", native: "عربي", code: "ar" },
  { name: "Armenian", native: "Հայերէն", code: "hy" },
  { name: "Azerbaijani", native: "آذربایجان دیلی", code: "az" },
  { name: "Basque", native: "Euskara", code: "eu" },
  { name: "Belarusian", native: "Беларуская", code: "be" },
  { name: "Bulgarian", native: "Български", code: "bg" },
  { name: "Catalan", native: "Català", code: "ca" },
  { name: "Chinese (Simplified)", native: "中文简体", code: "zh-CN" },
  { name: "Chinese (Traditional)", native: "中文繁體", code: "zh-TW" },
  { name: "Croatian", native: "Hrvatski", code: "hr" },
  { name: "Czech", native: "Čeština", code: "cs" },
  { name: "Danish", native: "Dansk", code: "da" },
  { name: "Dutch", native: "Nederlands", code: "nl" },
  { name: "English", native: "English", code: "en" },
  { name: "Estonian", native: "Eesti keel", code: "et" },
  { name: "Filipino", native: "Filipino", code: "tl" },
  { name: "Finnish", native: "Suomi", code: "fi" },
  { name: "French", native: "Français", code: "fr" },
  { name: "Galician", native: "Galego", code: "gl" },
  { name: "Georgian", native: "ქართული", code: "ka" },
  { name: "German", native: "Deutsch", code: "de" },
  { name: "Greek", native: "Ελληνικά", code: "el" },
  { name: "Haitian Creole", native: "Kreyòl ayisyen", code: "ht" },
  { name: "Hebrew", native: "עברית", code: "iw" },
  { name: "Hindi", native: "हिन्दी", code: "hi" },
  { name: "Hungarian", native: "Magyar", code: "hu" },
  { name: "Icelandic", native: "Íslenska", code: "is" },
  { name: "Indonesian", native: "Bahasa Indonesia", code: "id" },
  { name: "Irish", native: "Gaeilge", code: "ga" },
  { name: "Italian", native: "Italiano", code: "it" },
  { name: "Japanese", native: "日本語", code: "ja" },
  { name: "Korean", native: "한국어", code: "ko" },
  { name: "Latvian", native: "Latviešu", code: "lv" },
  { name: "Lithuanian", native: "Lietuvių kalba", code: "lt" },
  { name: "Macedonian", native: "Македонски", code: "mk" },
  { name: "Malay", native: "Malay", code: "ms" },
  { name: "Maltese", native: "Malti", code: "mt" },
  { name: "Norwegian", native: "Norsk", code: "no" },
  { name: "Persian", native: "فارسی", code: "fa" },
  { name: "Polish", native: "Polski", code: "pl" },
  { name: "Portuguese", native: "Português", code: "pt" },
  { name: "Romanian", native: "Română", code: "ro" },
  { name: "Russian", native: "Русский", code: "ru" },
  { name: "Serbian", native: "Српски", code: "sr" },
  { name: "Slovak", native: "Slovenčina", code: "sk" },
  { name: "Slovenian", native: "Slovensko", code: "sl" },
  { name: "Spanish", native: "Español", code: "es" },
  { name: "Swahili", native: "Kiswahili", code: "sw" },
  { name: "Swedish", native: "Svenska", code: "sv" },
  { name: "Thai", native: "ไทย", code: "th" },
  { name: "Turkish", native: "Türkçe", code: "tr" },
  { name: "Ukrainian", native: "Українська", code: "uk" },
  { name: "Urdu", native: "اردو", code: "ur" },
  { name: "Vietnamese", native: "Tiếng Việt", code: "vi" },
  { name: "Welsh", native: "Cymraeg", code: "cy" },
  { name: "Yiddish", native: "ייִדיש", code: "yi" },
    ],
    };
  },
  computed: {
    inputLanguageTitle() {
      return `${this.inputLanguage.name} (${this.inputLanguage.native})`;
    },
    outputLanguageTitle() {
      return `${this.outputLanguage.name} (${this.outputLanguage.native})`;
    },
  },
  methods: {
    toggleDetails(index) {
      this.expandedIndex = this.expandedIndex === index ? null : index;
    },
    toggleDropdown(type) {
      if (type === 'input') {
        this.inputDropdownActive = !this.inputDropdownActive;
        this.outputDropdownActive = false; 
      } else if (type === 'output') {
        this.outputDropdownActive = !this.outputDropdownActive;
        this.inputDropdownActive = false; 
      }
    },
    selectInputLanguage(language) {
      this.inputLanguage = language;
      this.inputDropdownActive = false;
      // this.translate();
    },
    selectOutputLanguage(language) {
      this.outputLanguage = language;
      this.outputDropdownActive = false;
      // this.translate();
    },
    limitInput() {
      if (this.inputText.length > 5000) {
        this.inputText = this.inputText.slice(0, 5000);
      }
      // this.translate();
    },
    speakText() {
      if (!this.outputText.trim()) {
        alert('Enter something to translate first.');
        return; // If the textarea is empty or just contains spaces, do nothing
      } 
      if ('speechSynthesis' in window) {
  const utterance = new SpeechSynthesisUtterance(this.outputText);

  // Set pitch, rate, and volume
  utterance.pitch = 1;  // Range: 0 (lowest) to 2 (highest)
  utterance.rate = 1;   // Range: 0.1 (slowest) to 10 (fastest)
  utterance.volume = 1; // Range: 0 (silent) to 1 (loudest)

  // Get the list of available voices
  const voices = window.speechSynthesis.getVoices();

  // Optionally, set a preferred voice
  const preferredVoice = voices.find(voice => voice.lang === 'en-US' && voice.name.includes('Google')); 
  if (preferredVoice) {
    utterance.voice = preferredVoice; // Use the preferred voice
  } else {
    console.warn('Preferred voice not available, using default voice.');
  }

  // Add event listeners for better control and error handling
  utterance.onstart = () => {
    console.log('Speech synthesis started');
  };

  utterance.onend = () => {
    console.log('Speech synthesis finished');
  };

  utterance.onerror = (event) => {
    console.error('Speech synthesis error:', event.error);
  };

  // Speak the text
  window.speechSynthesis.speak(utterance);
  
} else {
  alert('Sorry, your browser does not support Text-to-Speech.');
}

    },
    translate() {
      if (!this.inputText.trim()) {
        alert('Enter some Text first!');
        return; // Don't make the API call if there's no text
      }
      const inputLanguage = this.inputLanguage.code;
      const outputLanguage = this.outputLanguage.code;
      const url = `https://translate.googleapis.com/translate_a/single?client=gtx&sl=${inputLanguage}&tl=${outputLanguage}&dt=t&q=${encodeURIComponent(this.inputText)}`;

      fetch(url)
        .then((response) => response.json())
        .then((json) => {
          this.outputText = json[0].map((item) => item[0]).join("");
          this.addToHistory(this.inputText, this.outputText, this.inputLanguage.name, this.outputLanguage.name);

        })
        .catch((error) => {
          console.log(error);
        });
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.uploadedFileName = file.name;
        const reader = new FileReader();
        reader.readAsText(file);
        reader.onload = (e) => {
          this.inputText = e.target.result;
          // this.translate();
        };
        reader.onerror = (e) => {
          alert('Error reading file.');
        };
        event.target.value = ''; // Reset file input
      } else {
        alert('Please upload a valid text file (TXT).');
      }
    },
    swapLanguages() {
      const tempLanguage = this.inputLanguage;
      this.inputLanguage = this.outputLanguage;
      this.outputLanguage = tempLanguage;

      const tempText = this.inputText;
      this.inputText = this.outputText;
      this.outputText = tempText;

      // this.translate();
    },
    downloadTranslatedFile() {
      if (!this.outputText.trim()) {
        alert('Enter something to translate first.');
        return; // If the textarea is empty or just contains spaces, do nothing
      }
      const blob = new Blob([this.outputText], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.download = `translated-to-${this.outputLanguage.code}.txt`;
      a.href = url;
      a.click();
    },



    //HISTORY
    addToHistory(inputText, outputText, inputLanguage, outputLanguage) {
      const newEntry = {
        inputText,
        outputText,
        inputLanguage,
        outputLanguage,
        timestamp: new Date().toLocaleString('en-US', { timeZone: 'Asia/Kolkata' }),
        
      };
      this.history.push(newEntry);

      // Limit history to 10 entries
      if (this.history.length > 10) {
        this.history.shift(); // Remove the oldest entry
      }
      localStorage.setItem('translationHistory', JSON.stringify(this.history));
      this.showSnackbar('Translation saved to history');
    },
    deleteHistory(index) {
      this.history.splice(index, 1);
      localStorage.setItem('translationHistory', JSON.stringify(this.history));
      this.showSnackbar('History entry deleted');
    },
    showSnackbar(message) {
      this.snackbarMessage = message;
      setTimeout(() => {
        this.snackbarMessage = '';
      }, 3500);
    },

  },
  mounted() {
    const savedHistory = localStorage.getItem('translationHistory');
    if (savedHistory) {
      this.history = JSON.parse(savedHistory);
    }
  },
};
</script>