<template>
  <div>
    <div class="words">
      <transcript v-for="(word, i) in transcribedWords" :key="i" :word="word" :index="i"
        :current="i + 1 === transcribedWords.length" />

    </div>
    <div class="footer">
      <img v-if="!speech" @click="startVoiceRecognition()" style="width: 32px; height: 32px; cursor: pointer"
        src="../assets/mic_on.svg" alt="">
      <img v-else @click="startVoiceRecognition('stop')" style="width: 32px; height: 32px; cursor: pointer"
        src="../assets/mic_off.svg" alt="">
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import transcript from '@/components/transcript.vue'
export default {
  name: 'Home',
  components: {
    HelloWorld,
    transcript
  },
  data() {
    return {
      speech: false,
      transcribedWords: []
    }
  },
  methods: {
    startVoiceRecognition(action) {
      window.SpeechRecognition = window.SpeechRecognition
        || window.webkitSpeechRecognition;

      const recognition = new SpeechRecognition();
      recognition.interimResults = true;
      recognition.lang = 'en-US';
      recognition.continuous = true;

      this.speech = true
      if (action === 'stop') {
        this.speech = false
        recognition.stop()
        return
      }
      
      recognition.onresult = (event) => {
        const current = event.resultIndex;
        const transcript = event.results[current][0].transcript;
        if (event.results[current].isFinal && this.speech) {
          this.transcribedWords.push(transcript);
        }
      }

      recognition.onstart = () => {
        console.log('Voice recognition activated. Try speaking into the microphone.');
      }

      recognition.onend = () => {
        console.log('Voice recognition ended.');
      }
      recognition.start()
    }
  },
  watch: {
    "transcribedWords.length"(newVal) {
      if (newVal > 6) {
        this.transcribedWords.shift()
      }
    }
  },
  mounted() { }
}
</script>
<style lang="scss">
.words {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  // background: black;
  height: auto;
}

#p {
  margin: 0 auto;
  color: white;
}

.footer {
  width: 100vw;
  height: auto;
  position: fixed;
  top: auto;
  left: 0;
  bottom: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(20px);
  border-radius: 8px;
  height: 80px;
  padding-top: 2%;
}
</style>
