<template>
  <div class="flex justify-center items-center h-screen">
    <div>
      <div class="rounded border border-gray-300 h-16 mb-4 relative">
        <div class="absolute right-0 top-0 text-xs p-2">
          {{ secondaryText }}
        </div>
        <div class="absolute right-0 bottom-0 text-xl p-2">
          {{ mainText }}
        </div>
      </div>
      <KeyBoard :keys="keys" @key-pressed="handleKey($event)" />

    </div>

  </div>

</template>

<script>
import KeyBoard from './components/KeyBoard.vue'

export default {
  name: 'App',
  components: {
    KeyBoard
  },
  data() {
    return {
      keys: [],
      operation: '0',
      lastOperation: null,
      result: null
    }
  },
  created() {
    this.initKeys()
  },
  mounted() {
    window.addEventListener('keydown', this.handleKey)
  },
  unmounted() {
    window.removeEventListener('keydown', this.handleKey)
  },
  computed: {
    mainText() {
      return this.result && !this.operation ? this.result : this.operation
    },
    secondaryText() {
      if (this.result && !this.operation) {
        return this.lastOperation + " = "
      }
      else if (this.result && this.operation && this.operation !== '0' || this.result && this.operation === '0' && this.lastOperation) {
        return "Ans: " + this.result
      }
      else if (this.operation && this.operation !== '0') {
        return "Ans: 0"
      }
      return ""
    }
  },
  watch: {
    operation(value) {
      this.keys = this.keys.map(key => {
        if (key.key === 'AC' || key.key === 'CE') {
          if (value === '0' || this.result && !this.operation) {
            key.key = 'AC'
          } else {
            key.key = 'CE'
          }

        }
        return key
      })

    }
  },
  methods: {
    initKeys() {
      this.keys = [
        {
          key: '(',
          color: 'dark'
        },
        {
          key: ')',
          color: 'dark'
        },
        {
          key: '%',
          color: 'dark'

        },
        {
          key: 'AC',
          color: 'dark'
        },
        {
          key: '7',
          color: 'light'

        },
        {
          key: '8',
          color: 'light'

        },
        {
          key: '9',
          color: 'light'

        },
        {
          key: '/',
          color: 'dark'

        },
        {
          key: '4',
          color: 'light'

        },
        {
          key: '5',
          color: 'light'

        },
        {
          key: '6',
          color: 'light'

        },
        {
          key: 'x',
          color: 'dark'

        },
        {
          key: '1',
          color: 'light'

        },
        {
          key: '2',
          color: 'light'

        },
        {
          key: '3',
          color: 'light'

        },
        {
          key: '-',
          color: 'dark'

        },
        {
          key: '0',
          color: 'light'

        },
        {
          key: '.',
          color: 'light'

        },
        {
          key: '=',
          color: 'blue'

        },
        {
          key: '+',
          color: 'dark'

        },
      ]
    },
    handleKey(key) {
      if (!this.operation && this.result && this.isAOperator(key)) {
        this.operation = String(this.result)
      }
      switch (key.key) {
        case 'Enter':
          this.calculate()
          break
        case 'Backspace':
          this.eraseFromOperation()
          break
        default:
          if (key.key === '=') {
            this.calculate()
          }
          else if (key.key === 'AC' || key.key === 'CE') {
            this.eraseFromOperation()
          }
          else if (this.isANumber(key) || this.isAOperator(key) && (this.lastOperationCharIsANumber() || this.lastOperationCharIsPercentage())) {
            this.addToOperation(key)
          }
          else if (this.isAZero(key) && this.lastOperationCharIsANumber()) {
            this.addToOperation(key)
          }

          else if (key.key === '(') {
            this.addToOperation(key)
          }

          else if ((key.key === '.' || key.key === ',') && !this.alreadyHasADot()) {
            this.addToOperation(key)
          }

      }
    },
    alreadyHasADot() {
      for (let i = this.operation.length - 1; i >= 0; i--) {
        if (this.isAOperator({
          key: this.operation[i]
        })) {
          break;
        }
        if (this.operation[i] === '.' || this.operation[i] === ',') {
          return true;
        }
      }
      return false
    },
    isANumber(key) {
      return key.key >= 1 && key.key <= 9
    },
    isAZero(key) {
      return key.key === '0'
    },
    isAOperator(key) {
      return key.key === '-' || key.key === '+' || key.key === '*' || key.key === 'x' || key.key === '/'
    },

    eraseFromOperation() {
      if (this.operation && this.operation.length > 0 && this.operation !== '0') {
        this.operation = this.operation.slice(0, -1)
      } else {
        this.operation = '0'
      }
    },
    addToOperation(key) {
      if (this.operation === '0' || !this.operation) {
        this.operation = ''
      }
      this.operation += this.getChar(key)
    },
    getChar(key) {
      if (key.key === 'x') {
        return '*'
      }
      else if (key.key === ',') {
        return '.'
      }
      return key.key
    },
    lastOperationCharIsANumber() {
      return this.operation && this.operation.length > 0 && this.operation[this.operation.length - 1] > 0 && this.operation[this.operation.length - 1] <= 9
    },
    lastOperationCharIsZero() {
      return this.operation && this.operation.length > 0 && this.operation[this.operation.length - 1] === '0'
    },

    calculate() {
      if (!this.operation || !this.lastOperationCharIsANumber()) {
        return
      }
      this.operation = this.parseOperation()
      this.result = eval(this.operation)
      this.lastOperation = this.operation
      this.operation = null
    },
    parseOperation() {
      let parsedOperation = '';
      let openParenthesis = 0;
      for (let i = 0; i < this.operation.length; i++) {
        if (this.operation[i] === '(') {

          openParenthesis++

        }
        else if (this.operation[i] === ')') {
          openParenthesis--
        }

        parsedOperation += this.operation[i]
      }

      for (let i = 0; i < openParenthesis; i++) {
        parsedOperation += ')'

      }

      console.log(parsedOperation)
      return parsedOperation
    }


  }
}
</script>

<style>
</style>
