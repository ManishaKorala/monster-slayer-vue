<template>
  <v-container>
    <v-row>
      <v-col cols="6">
        <h1 class="text-center">{{ playerName }}</h1>
        <div class="healthbar">
          <div
                  class="healthbar text-center"
                  style="background-color: green; margin: 0; color: white;"
                  :style="{width: playerHealth + '%'}">
            {{ playerHealth }}
          </div>
        </div>
      </v-col>
      <v-col cols="6">
        <h1 class="text-center">MONSTER</h1>
        <div class="healthbar">
          <div
                  class="healthbar text-center"
                  style="background-color: green; margin: 0; color: white;"
                  :style="{width: monsterHealth + '%'}">
            {{ monsterHealth }}
          </div>
        </div>
      </v-col>
    </v-row>
    <div class="row controls" v-if="!gameIsRunning">
      <v-col cols="12">
        <button id="start-game" @click="startGame">START NEW GAME</button>
      </v-col>
    </div>
    <v-row class="controls" v-else>
      <v-col cols="12">
        <button id="attack" @click="attack">ATTACK</button>
        <button id="special-attack" @click="specialAttack">SPECIAL ATTACK</button>
        <button id="heal" @click="heal">HEAL</button>
        <button id="give-up" @click="giveUp">GIVE UP</button>
      </v-col>
    </v-row>
    <v-row class="log" v-if="turns.length > 0">
      <v-col cols="12">
        <ul>
          <li v-for="turn in turns"
              :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}"
              :key="turn.id">
            {{ turn.text }}
          </li>
        </ul>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'FightMonster',
    data: () => ({
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false,
      turns: [],
      playerName: '',
      currentTurn: 0
    }),
    created() {
      this.playerName = window.prompt("Enter your name: ");
    },
    methods: {
      startGame () {
        this.gameIsRunning = true;
        this.playerHealth = 100;
        this.monsterHealth = 100;
        this.turns = [];
      },
      attack () {
        let damage = this.calculateDamage( 10);
        this.monsterHealth -= damage;
        this.turns.unshift({
          isPlayer: true,
          text: 'Player hits Monster for ' + damage,
          id: this.currentTurn + 1
        });
        this.currentTurn++;
        this.monsterAttacks();
        this.checkWin();
      },
      specialAttack () {
        let damage = this.calculateDamage(15);
        this.monsterHealth -= damage;
        this.turns.unshift({
          isPlayer: true,
          text: 'Player hits Monster hard for ' + damage,
          id: this.currentTurn + 1
        });
        this.currentTurn++;
        this.monsterAttacks();
      },
      heal () {
        if (this.playerHealth <= 90) {
          this.playerHealth += 10;
        } else {
          this.playerHealth = 100;
        }
        this.turns.unshift({
          isPlayer: true,
          text: 'Player heals for 10',
          id: this.currentTurn + 1
        });
        this.currentTurn++;
        this.monsterAttacks();
      },
      giveUp () {
        this.gameIsRunning = false;
      },
      monsterAttacks () {
        let damage = this.calculateDamage(12);
        this.playerHealth -= damage;
        this.turns.unshift({
          isPlayer: false,
          text: 'Monster hits Player for ' + damage,
          id: this.currentTurn + 1
        });
        this.checkWin();
        this.currentTurn++;
      },
      calculateDamage (max) {
        return Math.round(Math.random() * max) + 1;
      },
      displayResult (result) {
        if (confirm(`You ${result}! New Game?`)) {
          this.startGame();
        } else {
          this.gameIsRunning = false;
        }
      },
      checkWin () {
        if (this.monsterHealth <= 0) {
          this.displayResult ('won')
        } else if (this.playerHealth <= 0) {
          this.displayResult ('lost')
        }
      }
    }
  }
</script>
<style scoped>
  .text-center {
    text-align: center;
  }

  .healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
  }

  .controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
  }

  .turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
  }

  .log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
  }

  .log ul li {
    margin: 5px;
  }

  .log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
  }

  .log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
  }

  button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
  }

  #start-game {
    background-color: #aaffb0;
  }

  #start-game:hover {
    background-color: #76ff7e;
  }

  #attack {
    background-color: #ff7367;
  }

  #attack:hover {
    background-color: #ff3f43;
  }

  #special-attack {
    background-color: #ffaf4f;
  }

  #special-attack:hover {
    background-color: #ff9a2b;
  }

  #heal {
    background-color: #aaffb0;
  }

  #heal:hover {
    background-color: #76ff7e;
  }

  #give-up {
    background-color: #ffffff;
  }

  #give-up:hover {
    background-color: #c7c7c7;
  }
</style>