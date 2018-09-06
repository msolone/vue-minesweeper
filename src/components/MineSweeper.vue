<template>
<section>
    <h3>{{ message}}</h3>
    <table class="game_board">
        <tbody>
            <tr v-for="(row, i) in board" v-bind:key="i">
                <td class="squares" v-for="(col, j) in row"
                 v-bind:key="j" 
                 v-on:click="squareClicked(i,j)"
                 v-on:contextmenu="event => squareRightClicked(event, i,j)"
                 >
                        {{ col }}
                </td>
            </tr>
        </tbody>
    </table>
</section>

</template>

<script>
export default {
  name: "MineSweeper",
  data: function() {
    return {
      board: [],
      gameId: 0,
      message: "Let's Play MineSweeper"
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
        body: JSON.stringify({ difficulty: 0 }),
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
    },
    squareRightClicked(event, i, j) {
        
      event.preventDefault();
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
          console.log(latestGameData)
           this.board = latestGameData.board
          });    
    }
  }
};
</script>

<style scoped>
td {
  border: 3px solid black;
  height: 2em;
  width: 2em;
}
</style>
