<template>
  <div class="point container">
    Best Score
    <img class="coin flipper" style="width: 30px" src="../assets/coin.svg" />
    {{ total }}
  </div>
  <span v-if="!show" class="point container">
    Accuracy Rate {{ percent }} %</span
  >
  <div class="container point" v-if="!show">
    <button
      class="newGame"
      v-on:click="(minute = 5), (second = 59), (points = 0), countDown()"
    >
      New Game
    </button>
  </div>

  <div class="center">
    <div class="container">
      <span class="counter">
        0{{ minute }}:
        <span :class="big ? 'countDown' : ''">{{ second }} </span>
      </span>

      <span class="point container">
        <img class="coin flipper" src="../assets/coin.svg" /> {{ points }}
      </span>
    </div>

    <div class="container">
      <p
        oncopy="return false"
        oncut="return false"
        onpaste="return false"
        v-for="item in setWords"
        :key="item.id"
      >
        {{ item }}
      </p>
    </div>

    <input v-if="show" v-model="word" ref="grs" />
  </div>
</template>

<script>
import words from "./words.json";
export default {
  data() {
    return {
      correct: [],
      wrong: [],
      show: true,
      second: 30,
      minute: 1,
      word: "",
      setWords: [],
      words: words.map((x) => x.toLowerCase()),
      points: 0,
      big: false,
      percent: 0,
      total: localStorage.getItem("score"),
    };
  },
  created() {
    for (let i = 0; i < 5; i++) {
      this.setWords.push(
        this.words[Math.floor(Math.random() * this.words.length)]
      );
    }
    this.$refs.grs.focus();

    document.addEventListener("keydown", (e) => {
      if (e.code === "Space") {
        e.returnValue = false;
        this.wordInput();
        this.word = "";
      }
    });

    window.addEventListener(
      "contextmenu",
      function (e) {
        e.preventDefault();
      },
      false
    );

    this.countDown();
  },

  methods: {
    newWord() {
      this.setWords.push(
        this.words[Math.floor(Math.random() * this.words.length)]
      );
    },

    countDown() {
      setTimeout(() => {
        this.second -= 1;
        this.show = true;
        if (this.minute === 0 && this.second === 0) {
          alert("Tüh! Süreniz Bitti! Yazık...");
          this.word = "";
          this.show = false;

          if (this.correct.values < 0) {
            this.percent = 0;
          } else {
            let toplam = this.correct.length + this.wrong.length;
            this.percent = Math.floor((100 * this.correct.length) / toplam);
            if (this.points > this.total) {
              localStorage.setItem("score", this.points);
              this.total = this.points;
              this.points = 0;
            }
          }
        } else if (this.minute === 0 && this.second < 10) {
          if (this.second % 2 === 0) {
            this.big = true;
          } else {
            this.big = false;
          }

          this.countDown();
        } else if (this.second === 0) {
          this.minute -= 1;
          this.second = 59;
          this.countDown();
        } else {
          this.countDown();
        }
      }, 1000);
    },

    wordInput() {
      if (this.word === this.setWords[0]) {
        this.correct.push(this.word);
        this.points += 10;
        this.setWords.shift();
        this.newWord();
      } else {
        this.wrong.push(this.word);
        this.setWords.shift();
        this.newWord();
      }
    },
  },
};
</script>


<style scoped>
@font-face {
  font-family: "arcade";
  src: url("../assets/font.ttf") format("truetype");
}

.center {
  margin-top: 5% !important;
  border: 1px solid #ccc;
  width: 50%;
  height: 500px;
  background: wheat;
  border-radius: 10px;
  margin-left: 450px;
  text-align: center;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.container p {
  margin: 10px;
  margin-top: 100px;
  opacity: 0.9;
}

.counter {
  margin-right: 600px;
  font-family: arcade;
  font-size: 2em;
}

.container p:first-child {
  font-size: 6em;
  opacity: 1;
}

input {
  border: 5px solid red;
  height: 30px;
  width: 600px;
  font-size: 1.5em;
}

input:focus {
  border: 5px solid green;
}

.point {
  font-family: arcade;
  font-size: 2em;
}

.flipper {
  transition: 2s;
  transform-style: preserve-3d;
}
.coin {
  animation: rotation 1s infinite linear;
  margin: 10px;
}
@keyframes rotation {
  100% {
    transform: rotatey(360deg);
  }
}

.newGame {
  margin-top: 20px;
  font-size: 1em;
  font-family: arcade;
  color: #fff;
  background: grey;
  border-radius: 20px;
}

.countDown {
  font-size: 1.2em;
}
</style>

<!-- Add "scoped" attribute to limit CSS to this component only -->

