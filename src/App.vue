<template>
<div id="app">
  <button @click="newGame()">
    NOUVELLE PARTIE
  </button>
  <hr>
  <h4>{{ score }}</h4>
  <hr>
  <div>
    <span class="carre" @click="selectSquare('hautGauche')" :class="{ bleu: hautGauche }"></span>
    <span class="carre" @click="selectSquare('hautDroite')" :class="{ rouge: hautDroite }"></span>
  </div>
  <div>
    <span class="carre" @click="selectSquare('basGauche')" :class="{ vert: basGauche }"></span>
    <span class="carre" @click="selectSquare('basDroite')" :class="{ jaune: basDroite }"></span>
  </div>
  <hr>
  <span>sequence : </span>
  <pre>{{ sequence }}</pre>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      hautGauche: false,
      hautDroite: false,
      basGauche: false,
      basDroite: false,
      sequence: [],
      tmp: [],
      squareMapping: ['hautGauche', 'hautDroite', 'basGauche', 'basDroite']
    }
  },
  computed: {
    score() {
        const value = this.sequence.length - 1
        return (value < 0) ? `Score : 0` : `Score : ${ value }`
    }
  },
  methods: {
      addNewElemToSequence() {
          this.sequence.push(this.squareMapping[Math.floor(Math.random() * 4)])
          this.tmp = this.sequence.slice() // copie de l'array sequence dans l'array tmp
      },
      allGray() {
          this.hautGauche = false
          this.hautDroite = false
          this.basGauche = false
          this.basDroite = false
      },
      newGame() {
          this.sequence = []
          this.nextTurn()
      },
      nextTurn() {
          this.addNewElemToSequence()
          this.allGray()
          this.playSequence(this.tmp[0])
      },
      playSequence(instruction) {
          this[instruction] = true
          var _this = this;
          setTimeout(function() {
              _this.allGray() // _this à la place de this (this non reconnu dans une fonction dans une autre fonction)
              _this.tmp.shift() // on décalle le contenu du tableau : le premer élément disparait
              if (_this.tmp[0]) {
                   setTimeout(function() {
                       _this.playSequence(_this.tmp[0])
                  }, 400)
              } else {
                  _this.tmp = _this.sequence.slice()
              }
          }, 400)
      },
      selectSquare(carre) {
          console.log(carre)
          if(carre === this.tmp[0]) {
              // console.log('ok')
              this[carre] = true
              var _this = this;
              setTimeout(function() {
                  _this.allGray()
                  _this.tmp.shift()
                  if (!_this.tmp[0]) {
                      _this.nextTurn()
                  }
              }, 400)
          } else {
              // console.log('ko')
              alert('perdu')
          }
      }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.carre {
  display: inline-block;
  width: 200px;
  height: 200px;
  background-color: #ccc;
  cursor: pointer;
}

.bleu {
  background-color: blue;
}

.rouge {
  background-color: red;
}

.vert {
  background-color: green;
}

.jaune {
  background-color: yellow;
}
</style>
