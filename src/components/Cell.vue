<template>
	<td class="cell" @click="strike">{{ mark }}</td>
</template>

<script>
	export default {
    props: ['name'],
    name: 'cell',
		data () {
			return {
				// enables the player to place a mark
 				frozen: false,
				// holds either X or O to be displayed in the td
				mark: ''
			}	
		},

		methods: {
			strike () {
				if (! this.frozen) {
					// gets either X or O from the Grid component
					this.mark = this.$parent.activePlayer

					this.frozen = true
					
					// fires an event to notify the Grid component that a mark is placed
					Event.$emit('strike', this.name)
				}
      },
      areEqual () {
            var len = arguments.length;

            // loops through each value and compares them with an empty sting and 
            // for inequality
            for (var i = 1; i < len; i++){
                if (arguments[i] === '' || arguments[i] !== arguments[i-1])
                    return false;
            }
            return true;
      },
		},

		created () {
			Event.$on('clearCell', () => {
				this.mark = ''

				this.frozen = false
			})

			Event.$on('freeze', () => this.frozen = true)
		}
	}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=PT+Sans&display=swap');
.cell {
  width: 33.333%;
  height: 90px;
  border: 6px solid white;
  font-size: 3.5em;
  font-family: 'PT Sans';
}


.cell::after {
  content: '';
  display: block;
}

.cell:first-of-type {
  border-left-color: transparent;
  border-top-color: transparent;
}

.cell:nth-of-type(2) {
  border-top-color: transparent;
}

.cell:nth-of-type(3) {
  border-right-color: transparent;
  border-top-color: transparent;
}

tr:nth-of-type(3) .cell {
  border-bottom-color: red;
}
</style>