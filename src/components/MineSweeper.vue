<template>
<section>
    <h3>{{ message}}</h3>
    <section class="difficulty_buttons">
      <button v-on:click='easyMode'>Easy</button>
      <button v-on:click='normalMode'>Normal</button>
      <button v-on:click='hardMode'>Hard</button>
    </section>
    <button v-on:click="resetGame" class="reset_button">Reset Game</button>
    <table class="game_board">
        <tbody>
            <tr v-for="(row, i) in board" v-bind:key="i">
                <td class="squares" v-for="(col, j) in row"
                 v-bind:key="j" 
                 v-on:click="squareClicked(i,j)"
                 v-on:contextmenu="event => squareRightClicked(event, i,j)"
                 >
                        {{getSquareContent(col)}}
                        <!-- <Square :character="col.toString()"/> -->
                </td>
            </tr>
        </tbody>
    </table>
</section>

</template>

<script>
import Square from './Square.vue';
export default {
  name: "MineSweeper",
  components: {
    Square
  },
  data: function() {
    return {
      board: [],
      gameId: 0,
      message: "Let's Play MineSweeper",
      difficulty: 0
    };
  },
  mounted: function() {
    this.createGame();
  },
  methods: {
    createGame() {
      const BASE_URL = "https://minesweeper-api.herokuapp.com/";
      fetch(`${BASE_URL}games`, {
        method: "POST",
        body: JSON.stringify({ difficulty: this.difficulty }),
        headers: {
          "Content-Type": "application/json"
        }
      })
        .then(resp => resp.json())
        .then(newGame => {
          console.log("Game", newGame);
          this.board = newGame.board;
          this.gameId = newGame.id;
        });
    },
    squareClicked(i, j) {
      if (this.message !== "You Lose!" && this.message !== "You Win!") {
        console.log("row:", i, "col:", j);
        const BASE_URL = "https://minesweeper-api.herokuapp.com/";
        fetch(`${BASE_URL}games/${this.gameId}/check`, {
          method: "POST",
          body: JSON.stringify({ row: i, col: j }),
          headers: {
            "Content-Type": "application/json"
          }
        })
          .then(resp => resp.json())
          .then(latestGame => {
            console.log("Latest Game", latestGame);
            if (latestGame.state === "lost") {
              this.board = latestGame.board;
              this.message = "You Lose!";
            } else if (latestGame.state === "won") {
              this.board = latestGame.board;
              this.message = "You Win!";
            } else {
              this.board = latestGame.board;
              this.message = "Watch out for Bombs!";
            }
          });
      }
    },
    squareRightClicked(event, i, j) {
      event.preventDefault();
      if (this.message !== "You Lose!" && this.message !== "You Win!") {
        console.log("Right Click row:", i, "col:", j);
        const BASE_URL = "https://minesweeper-api.herokuapp.com/";
        fetch(`${BASE_URL}games/${this.gameId}/flag`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({ row: i, col: j })
        })
          .then(resp => resp.json())
          .then(latestGameData => {
            console.log(latestGameData);
            this.board = latestGameData.board;
          });
      }
    },
    resetGame() {
      this.createGame();
      this.message = "New Game";
    },
    getSquareContent(space) {
      //return space
      if (space === "F") {
        return '🚩';
      } else if (space === "*") {
      return '💣';
      } else {
        return space;
      }
    },
    easyMode() {
      this.difficulty = 0
      this.createGame()
      this.message = "Easy Mode";
    },
    normalMode() {
      this.difficulty = 1
      this.createGame()
      this.message = "Normal Mode";
    },
    hardMode() {
      this.difficulty = 2
      this.createGame()
      this.message = "Hard Mode";
    }
  }
};
</script>

<style scoped>
td {
  border: 2px solid #2c3e50;
  height: 2em;
  width: 2em;
}
.difficulty_buttons {
  display: flex;
  justify-content: center;
  padding: 1em;
}
.reset_button {
 margin: 1em;
}
.blue-num {
  color: blue;
  font-size: 2em;
}

.green-num {
  color: green;
  font-size: 2em;
}

.red-num {
  color: red;
  font-size: 2em;
}

.purple-num {
  color: purple;
  font-size: 2em;
}

.black-num {
  color: black;
  font-size: 2em;
}

.maroon-num {
  color: maroon;
  font-size: 2em;
}

.gray-num {
  color: gray;
  font-size: 2em;
}

.turquoise-num {
  color: turquoise;
  font-size: 2em;
}

.blank-square {
  background-color: antiquewhite;
}

.flag > span {
  font-size: 2.5em;
}
</style>
