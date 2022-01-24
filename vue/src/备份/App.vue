<template>
  <div class="container">
    <div>
      <h1>{{ wordList[index].word }}</h1>
    </div>
    <div>
      <p style="font-size: 14px">
        {{ wordList[index].chinese }}
      </p>
    </div>
    <div style="margin: 50px 0">
      <input
        class="myanswer"
        :class="
          wordList[index].word == wordconfirm
            ? 'green'
            : 'red'
        "
        type="text"
        v-model="wordInput"
        @input="confirmword"
      />
      <div>
        <h3 style="font-size: 2rem; line-height: 1">
          {{
            wordList[index].word == wordconfirm
              ? "✅"
              : "coding...."
          }}
        </h3>
      </div>
    </div>
    <div style="margin: auto">
      <audio
        style="width: 80%"
        :src="wordList[index].online"
        controls="controls"
        autoplay
      ></audio>
    </div>
    <div id="notice">
      <span> * 使用上下左右键即可切换单词 </span>
    </div>
  </div>
</template>
<script>
const words = require("./assets/words.json");

export default {
  data() {
    return {
      played: [],
      wordList: [],
      index: 0,
      wordconfirm: false,
      wordInput: "",
    };
  },
  created() {
    this.wordList = words;
    this.randomIndex();

    window.document.onkeydown = this.checkWords;
  },
  methods: {
    randomIndex() {
      const len = this.wordList.length;
      const random = Math.floor(Math.random() * len + 1);
      this.index = random;
      this.played.push(random);
    },
    checkWords(evt) {
      this.wordconfirm = "";
      switch (evt.code) {
        case "ArrowUp":
          this.randomIndex();
          break;
        case "ArrowDown":
          this.backWord();
          break;
        case "ArrowRight":
          this.randomIndex();
          break;
        case "ArrowLeft":
          this.backWord();
          break;
      }
    },
    async confirmword(e) {
      this.wordconfirm = e.target.value;

      if (
        this.wordList[this.index].word == this.wordconfirm
      ) {
        setTimeout(() => {
          this.randomIndex();
          this.wordInput = "";
        }, 2000);
      }
    },
    backWord() {
      let last = this.played.length - 2;
      if (last <= 0) this.index = this.played[0];
      if (last > 0) {
        this.index = this.played[last];
        this.played = this.played.slice(0, last + 1);
      }
    },
  },
};
</script>
<style>
body {
  background: rgb(237, 237, 237);
  text-align: center;
  font-family: "Gill Sans", "Gill Sans MT", Calibri,
    "Trebuchet MS", sans-serif;
  line-height: 2;
}
.container {
  background-image: linear-gradient(
    to top,
    rgb(238, 195, 201) 0%,
    #e7627d 20%,
    #b8235a 39%,
    #801357 51%,
    #3d1635 74%,
    #1c1a27 100%
  );
  color: white;
  padding: 20px 10px;
  border-radius: 10px;
  height: 90vh;
  box-shadow: 3px 3px 6px rgb(194, 194, 194);
}
.myanswer {
  width: 90%;
  height: 50px;
  border: none;
  border-radius: 10px;
  font-size: 30px;
  text-align: center;
  font-family: monospace;
  font-weight: 800;
}
.green {
  color: green;
}
.red {
  color: grey;
}
#notice {
  position: absolute;
  bottom: 30px;
  margin: auto;
  text-align: center;
  width: 90%;
  font-size: 0.5rem;
  color: rgba(255, 255, 255, 0.3);
}
</style>
