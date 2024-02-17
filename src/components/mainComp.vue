<template>
  <div class="container jumbotron mt-5">
    <h1 class="display-4">Hızlı Yazma Yarışması !</h1>
    <p class="lead">Ne kadar hızlı klavye kullandığını test et.</p>

    <hr class="my-4" />
    <div v-if="isFinished" class="alert alert-primary">
      <h1>Süre Bitti</h1>
      <div>
        Doğru sayısı : {{ trueCount }} <br /><br />
        Yanlış sayısı : {{ falseCount }}
      </div>

      <button @click="newGame" class="btn mt-5 btn-success btn-block btn-lg">
        YENİ OYUN
      </button>
    </div>

    <div v-else>
      <div class="card">
        <div class="card-body">
          <span
            v-for="(word, index) in words.filter((data, index) => index < 10)"
            :key="word"
            :class="index === 0 && writingWordControl"
            class="mx-2 fs-2 fw-bolder"
          >
            {{ word }}</span
          >
          <div class="input-group input-group-lg my-4">
            <input type="text" class="form-control" v-model="inputText" />
            <button
              class="btn btn-outline-secondary fw-bold text-black"
              disabled
              type="button"
            >
              {{ timer }} sn.
            </button>
            <button
              class="btn btn-outline-secondary"
              type="button"
              @click="getWords"
              :disabled="isStart"
            >
              <i class="fa-solid fa-arrows-rotate"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import wordList from "@/assets/words.json";
export default {
  data() {
    return {
      words: [],
      inputText: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 5,
      interval: false,
      isStart: false,
      isFinished: true,
      wordList: wordList,
    };
  },
  watch: {
    inputText(e) {
      if (!this.isStart) {
        this.timerToggle();
      }
      let word = this.words[0].slice(0, e.length).toUpperCase();
      let userWord = e.replace(" ", "").toUpperCase();
      this.isTrue = word === userWord;

      if (e.indexOf(" ") !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++;
        this.words.splice(0, 1);
        this.inputText = "";
        this.isTrue = true;
      }
    },
  },

  computed: {
    writingWordControl() {
      return this.isTrue ? "writing-word" : "writing-word bg-danger";
    },
  },
  mounted() {
    this.getWords();
  },
  methods: {
    getWords() {
      this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300);
    },
    newGame() {
      this.getWords();
      this.isFinished = false;
      this.timer = 60;
      this.isTrue = true;
      this.isStart = false;
    },

    timerToggle() {
      this.isStart = true;
      this.interval = setInterval(this.timeProcess, 1000);
    },
    timeProcess() {
      if (this.timer === 1) {
        clearInterval(this.interval);
        this.isFinished = true;
        return;
      }
      this.timer--;
    },
  },
};
</script>

<style>
.writing-word {
  background-color: #aaa;
  border-radius: 15px;
  padding: 7px;
}
</style>