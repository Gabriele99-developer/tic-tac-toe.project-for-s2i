<template>
<div>
    <div class="gameStatus" :class="gameStatusColor">
        {{ gameStatusMessage }}
    </div>
    <table class="grid">
    <tr>
        <cell name="1"></cell>
        <cell name="2"></cell>
        <cell name="3"></cell>
    </tr>
    <tr>
        <cell name="4"></cell>
        <cell name="5"></cell>
        <cell name="6"></cell>
    </tr>
    <tr>
        <cell name="7"></cell>
        <cell name="8"></cell>
        <cell name="9"></cell>
    </tr>
    </table>
    </div>
</template>

<script>
import Cell from './Cell.vue'

export default {
    components: { Cell },
    data () {
        return {
            
            activePlayer: 'X',
            
            gameStatus: 'turn',

            gameStatusMessage: `Il turno di X`,

            gameStatusColor: 'statusTurn',
            // no. di mosse giocate da entrambi i giocatori in una singola partita (max = 9)
            moves: 0,
            // memorizza il posizionamento di X e O nelle celle in base al loro numero di cella
            cells: {
                1: '', 2: '', 3: '',
                4: '', 5: '', 6: '',
                7: '', 8: '', 9: ''
            },
            
            winConditions: [
                [1, 2, 3], [4, 5, 6], [7, 8, 9],
                [1, 4, 7], [2, 5, 8],    [3, 6, 9], 
                [1, 5, 9], [3, 5, 7]             
            ],
        }
    },
    computed: {
        // proprietà helper per ottenere il giocatore non attivo
        nonActivePlayer () {
            if (this.activePlayer === 'O') {
                return 'X'
            }

            return 'O'
        },
    },
    methods: {
        changePlayer () {
            this.activePlayer = this.nonActivePlayer
        },
        areEqual () {
                var len = arguments.length;

                // scorre ogni valore e li confronta con una puntura vuota e
                // per la disuguaglianza
                for (var i = 1; i < len; i++){
                    if (arguments[i] === '' || arguments[i] !== arguments[i-1])
                        return false;
                }
                return true;
        },
        checkForWin () {
            for (let i = 0; i < this.winConditions.length; i++) {
                // ottiene una singola condizione wc dall'intero array
                let wc = this.winConditions[i]
                let cells = this.cells

                // confronta i valori di 3 celle in base alle celle nella condizione
                if (this.areEqual(cells[wc[0]], cells[wc[1]], cells[wc[2]])) {
                    return true
                }
            }

            return false
        },
        
        gameIsWon () {
            // incendi vince l'evento per il componente App per modificare il punteggio
            Event.$emit('win', this.activePlayer)

                // imposta il messaggio di stato del gioco
                this.gameStatusMessage = `${this.activePlayer} Vittoria !`

                // attiva un evento per il congelamento della cella
                Event.$emit('freeze')

            // imposta lo stato per vincere
            return 'win'
        },
        changeGameStatus () {
            if (this.checkForWin()) {
                return this.gameIsWon()
            // controlla se la partita non è ancora vinta e tutte le celle sono piene
            } else if (this.moves === 9) {
                // imposta lo stato per disegnare
                return 'draw'
            }
            // imposta lo stato su svolta
            return 'turn'
        }
    },
    created () {
        Event.$on('gridReset', () => {
            Object.assign(this.$data, this.$options.data())
        }),
        Event.$on('strike', (cellNumber) => {
            // imposta X o O nella cella cliccata dell'array di celle
            this.cells[cellNumber] = this.activePlayer

            // incrementa il numero di mosse
            this.moves++

            // memorizza lo stato del gioco chiamando il metodo changeGameStatus
            this.gameStatus = this.changeGameStatus()

            if(this.gameStatus == 'turn') {
                this.changePlayer()
            }
        })
    },
    watch: {
        // osserva il cambiamento nel valore di gameStatus e cambia lo stato 
        // messaggio e colore di conseguenza
        gameStatus () {
            if (this.gameStatus === 'win') {
                this.gameStatusColor = 'statusWin'

                this.gameStatusMessage = `${this.activePlayer} Vittoria !`

                return
            } else if (this.gameStatus === 'draw') {
                this.gameStatusColor = 'statusDraw'

                this.gameStatusMessage = 'Pareggio!'

                return
            }

            this.gameStatusMessage = `${this.activePlayer} è il turno`
        }
    }
}
</script>

<style>
.grid {
  background-color: #34495e;
  color: #fff;
  width: 100%;
  border-collapse: collapse;
}

.gameStatus {
  margin: 0px;
  padding: 15px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background-color: #f1c40f;
  color: #fff;    
  font-size: 1.4em;
  font-weight: bold;
}

.statusTurn {
    background-color: green;
}

.statusWin {
    background-color: red;
}

.statusDraw {
    background-color: #9b59b6;
}

</style>