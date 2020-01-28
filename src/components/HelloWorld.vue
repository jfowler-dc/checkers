<template>
  <div class="flex">
    <div id="tiles">
      <div class="tile" @click="place(index)" v-for="(tile, index) in tiles" :class="tile.tileColor">
        <div class="puck" @click="select(index)" v-if="tile.puck == true" :class="tile.puckColor"><i v-if="tile.crown == 1" class="fas fa-crown"></i></div>
      </div>
    </div>
    <div class="side">
      <h1>Turn: <span v-if="turn == 0">Gray</span><span v-else>Red</span></h1>
      <div id="score">
        <div id="red">
          <h3>Red</h3>
          <h2>{{red}}</h2>
        </div>
        <div id="gray">
          <h3>Gray</h3>
          <h2>{{gray}}</h2>
        </div>
      </div>
      <span v-if="dangerous == true"><b>Danger, this application might break at this point...</b>
      </span>
      <hr v-if="dangerous == true">
      moves: {{ moves }} <br>
      selected: {{ selected }} <br>
      puck color: {{ currentPuckColor }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      tiles: [
        
      ],
      selected: '',
      placed: '',
      currentPuckColor: '',
      currentPuckStatus: '',
      moves: 0,
      red: 0,
      gray: 0, 
      turn: Math.round(Math.random()),
      dangerous: false
    }
  },
  methods: {
    createTiles() {
      for (var e = 0; e < 5; e++) {
        for(var i = 0; i < 10; i++) {
          if (i % 2 == 0) {
            this.tiles.push({puckColor: '', puck: false, tileColor: 'white', crown: 0})
          } else {
            if (e < 2 || e > 3) {
              if (e > 3) {
                this.tiles.push({puckColor: 'gray', puck: true, tileColor: 'black', crown: 0})
              } else {
                this.tiles.push({puckColor: 'red', puck: true, tileColor: 'black', crown: 0})
              }
            } else {
              this.tiles.push({puckColor: '', puck: false, tileColor: 'black', crown: 0})
            }
          }
        }
        for(var i = 0; i < 10; i++) {
          if (i % 2 == 0) {
            if (e < 1 || e > 2) {
              if (e > 2) {
                this.tiles.push({puckColor: 'gray', puck: true, tileColor: 'black', crown: 0})
              } else {
                this.tiles.push({puckColor: 'red', puck: true, tileColor: 'black', crown: 0})
              }
            } else {
              this.tiles.push({puckColor: '', puck: false, tileColor: 'black', crown: 0})
            }
          } else {
            this.tiles.push({puckColor: '', puck: false, tileColor: 'white', crown: 0})
          }
        }
      }
    },
    select(i) {
      this.selected = i;
      this.currentPuckColor = this.tiles[this.selected].puckColor;
      this.currentPuckStatus = this.tiles[this.selected].puck;
    },
    placeGray(puck, cur) {
      this.tiles[this.selected].puckColor = '';
      this.tiles[this.selected].puck = false;
      this.placed = cur;
      this.tiles[this.placed].puckColor = this.currentPuckColor;
      this.tiles[this.placed].puck = this.currentPuckStatus;

      if (this.tiles[this.selected].crown == 1) {
        this.tiles[this.placed].crown = 1;
      }

      if (this.tiles[this.selected - puck].puckColor === 'red') {
        this.gray = this.gray + 1;
        this.tiles[this.selected - puck].puckColor = '';
        this.tiles[this.selected - puck].puck = false;
        this.tiles[this.selected - puck].crown = 0;
      }

      this.turn = 1;
      this.moves = this.moves + 1;
    },
    placeGrayKing(puck, cur) {
      this.tiles[this.selected].puckColor = '';
      this.tiles[this.selected].puck = false;
      this.placed = cur;
      this.tiles[this.placed].puckColor = this.currentPuckColor;
      this.tiles[this.placed].puck = this.currentPuckStatus;
      this.tiles[this.placed].crown = 1;
      
      if (this.tiles[this.selected + puck].puckColor === 'red') {
        this.gray = this.gray + 1;
        this.tiles[this.selected + puck].puckColor = '';
        this.tiles[this.selected + puck].puck = false;
        this.tiles[this.selected - puck].crown = 0;
      }

      this.turn = 1;
      this.moves = this.moves + 1;
      this.dangerous = true;
    },
    placeRed(puck, cur) {
      this.tiles[this.selected].puckColor = '';
      this.tiles[this.selected].puck = false;
      this.placed = cur;
      this.tiles[this.placed].puckColor = this.currentPuckColor;
      this.tiles[this.placed].puck = this.currentPuckStatus;

      if (this.tiles[this.selected].crown == 1) {
        this.tiles[this.placed].crown = 1;
      }

      if (this.tiles[this.selected + puck].puckColor === 'gray') {
        this.red = this.red + 1;
        this.tiles[this.selected + puck].puckColor = '';
        this.tiles[this.selected + puck].puck = false;
        this.tiles[this.selected - puck].crown = 0;
      }

      this.turn = 0;
      this.moves = this.moves + 1;
    },
    placeRedKing(puck, cur) {
      this.tiles[this.selected].puckColor = '';
      this.tiles[this.selected].puck = false;
      this.placed = cur;
      this.tiles[this.placed].puckColor = this.currentPuckColor;
      this.tiles[this.placed].puck = this.currentPuckStatus;
      this.tiles[this.placed].crown = 1;

      if (this.tiles[this.selected - puck].puckColor === 'gray') {
        this.red = this.red + 1;
        this.tiles[this.selected - puck].puckColor = '';
        this.tiles[this.selected - puck].puck = false;
        this.tiles[this.selected - puck].crown = 0;
      }

      this.turn = 0;
      this.moves = this.moves + 1;
      this.dangerous = true;
    },
    place(i) {

      if (i != this.selected && this.tiles[i].puck == false) {

        // GRAY
        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected - 11) && this.tiles[i].crown == 0) {
          this.placeGray(11, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected - 9) && this.tiles[i].crown == 0) {
          this.placeGray(9, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected - 22) && this.tiles[i].crown == 0 && this.tiles[i + 11].puckColor == 'red') {
          this.placeGray(11, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected - 18) && this.tiles[i].crown == 0 && this.tiles[i + 9].puckColor == 'red') {
          this.placeGray(9, i)
        }


        // make gray king
        if (this.turn == 1 && this.currentPuckColor == 'gray' && this.tiles[i].crown == 0 && i == 1 || i == 3 || i == 5 || i == 7 || i == 9) {
          this.tiles[i].crown = 1
        }


        // gray king moves
        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected + 11) && this.tiles[this.selected].crown == 1) {
          this.placeGrayKing(11, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected + 9) && this.tiles[this.selected].crown == 1) {
          this.placeGrayKing(9, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected + 22) && this.tiles[this.selected].crown == 1 && this.tiles[i - 11].puckColor == 'red') {
          this.placeGrayKing(11, i)
        }

        if (this.turn == 0 && this.currentPuckColor == 'gray' && i == (this.selected + 18) && this.tiles[this.selected].crown == 1 && this.tiles[i - 9].puckColor == 'red') {
          this.placeGrayKing(9, i)
        }


        // RED
        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected + 11) && this.tiles[i].crown == 0) {
          this.placeRed(11, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected + 9) && this.tiles[i].crown == 0) {
          this.placeRed(9, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected + 22) && this.tiles[i].crown == 0 && this.tiles[i - 11].puckColor == 'gray') {
          this.placeRed(11, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected + 18) && this.tiles[i].crown == 0 && this.tiles[i - 9].puckColor == 'gray') {
          this.placeRed(9, i)
        }


        // make red king
        if (this.turn == 1 && this.currentPuckColor == 'red' && this.tiles[i].crown == 0 && i == 90 || i == 92 || i == 94 || i == 96 || i == 98) {
          this.tiles[i].crown = 1
        }


        // red king moves
        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected - 11) && this.tiles[this.selected].crown == 1) {
          this.placeRedKing(11, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected - 9) && this.tiles[this.selected].crown == 1) {
          this.placeRedKing(9, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected - 22) && this.tiles[this.selected].crown == 1 && this.tiles[i + 11].puckColor == 'gray') {
          this.placeRedKing(11, i)
        }

        if (this.turn == 1 && this.currentPuckColor == 'red' && i == (this.selected - 18) && this.tiles[this.selected].crown == 1 && this.tiles[i + 9].puckColor == 'gray') {
          this.placeRedKing(9, i)
        }
        

        // once a puck is placed, remove temporary variables
        this.selected = '';
        this.placed = '';
        this.currentPuckStatus = '';
        this.currentPuckColor = '';
      }
    }
  },
  created() {
    this.createTiles()
  }
}
</script>

<style scoped>
  #tiles {width:602px; margin:auto; margin-right:0; box-sizing:border-box; display:flex; flex-wrap:wrap; border:1px solid #000;}
  .tile {width:60px; height:60px; border:1px solid #000; display:flex; align-items:center; justify-content:center; box-sizing:border-box;}
  .puck {width:30px; height:30px; font-size:10px; border-radius:30px; display:flex; align-items:center; justify-content:center;}
  .white {background:#fff;}
  .black {background:#000;}
  .red {background:red;}
  .gray {background:#888;}
  .selected {box-sizing:border-box; box-shadow:0px 3px 6px rgba(0,0,0,.6);}
  .flex {display:flex; flex-wrap:wrap; align-items:center; height:100%}
  #score {display:flex; flex-wrap:wrap;}
  #red, #gray {width:50%;}
  .side {display:flex; flex-wrap:wrap; width:200px; flex-direction: column; margin:auto; margin-left:100px;}
  hr {width:100%;}
</style>
