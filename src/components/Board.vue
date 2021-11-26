<template>
  <div class="container">
    <div class="square-container">
      <div class="square" v-for="(data, index) in board_bg" :key="index"></div>
    </div>
    <div class="board-container">
      <div
        class="board"
        v-for="(data, index) in board"
        :key="index"
        @click="checker(index)"
      >
        <Stone :data="data" />
      </div>
    </div>
  </div>
  <div class="btn-container">
    <p>Next: {{ player === 1 ? "White" : "Black" }}</p>
    <p v-if="winner !== ''">{{ winner }}</p>
    <button class="btn" @click="restart()">Restart</button>
  </div>
</template>

<script>
import Stone from "./Stone.vue";

export default {
  name: "Board",
  components: {
    Stone,
  },
  data() {
    return {
      board_bg: Array(324).fill(0),
      board: Array(361).fill(0),
      player: 1,
      winner: "",
    };
  },
  methods: {
    checker(index) {
      if(this.winner !== ''){
        return ;  
      }
        this.board[index] === 0
          ? ((this.board[index] = this.player),
            this.player === 1 ? (this.player = 2) : (this.player = 1))
          : "",
        this.isWin(index) 
    },
    restart() {
      this.board.fill(0);
      this.player = 1;
      this.winner = "";
    },
    isWin(index) {
      let col = index % 19;
      let row = parseInt(index / 19);
      let left = col - 4;
      let right = col + 4;
      let top = row - 4;
      let bottom = row + 4;
      let result = "";
      for (let i = left; i <= right; i++) {
        if (i < 0 || i > 19) continue;
        result += this.board[row * 19 + i];
      }
      this.ifWin(result);
      result = "";
      for (let i = top; i <= bottom; i++) {
        if (i < 0 || i > 19) continue;
        result += this.board[i * 19 + col];
      }
      this.ifWin(result);
      result = "";
      for (let i = left, j = top; i <= right && j <= bottom; i ++, j ++){
       if( i < 0 || i > 19 || j < 0 || j > 19) continue;
       result += this.board[j * 19 + i];
      }
      this.ifWin(result);
      result = "";
      for (let i = left, j = bottom; i <= right && j >= top; i ++, j --){
       if( i < 0 || i > 19 || j < 0 || j > 19) continue;
       result += this.board[j * 19 + i];
      }
      this.ifWin(result);
      result = "";
    },
    ifWin(result) {
      if (result.includes("11111")) this.winner = "White Win";
      else if (result.includes("22222")) this.winner = "Black Win";
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 741px;
  position: relative;
  margin: auto;
}
.square-container {
  width: 740px;
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
  padding: 19px;
  border-radius: 10px;
  background: rgb(182, 123, 59);
}
.square {
  width: 40px;
  height: 40px;
  padding: 10px;
  border: 1px solid #000;
  background: rgb(182, 123, 59);
  margin-right: -1px;
  margin-bottom: -1px;
}
.board-container {
  width: 741px;
  display: flex;
  flex-wrap: wrap;
  position: absolute;
  top: 0px;
}
.board {
  display: flex;
  width: 39px;
  height: 39px;
}
.btn-container {
  max-width: 740px;
  margin: auto;
}
p {
  font-family: "Roboto Mono", monospace;
  margin: 20px;
  margin-bottom: 0;
  font-size: 1.125rem;
}
.btn {
  margin: 20px;
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  color: rgb(255, 255, 255);
  background: rgb(42, 165, 42);
  font-size: 1.05rem;
  cursor: pointer;
}
</style>
