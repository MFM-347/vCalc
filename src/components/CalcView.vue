<template>
  <div
    class="calculator"
    :style="{
      '--button-width': width,
      '--button-height': height,
      '--font': font
    }"
  >
    <div class="result" style="grid-area: result">{{ equation }}</div>
    <button style="grid-area: ac" @click="clear">AC</button>
    <button style="grid-area: plus-minus" @click="calculateToggle">±</button>
    <button style="grid-area: percent" @click="calculatePercentage">%</button>
    <button style="grid-area: add" @click="operate('+')">+</button>
    <button style="grid-area: subtract" @click="operate('-')">-</button>
    <button style="grid-area: multiply" @click="operate('×')">×</button>
    <button style="grid-area: divide" @click="operate('÷')">÷</button>
    <button style="grid-area: equal" @click="calculate">=</button>
    <button style="grid-area: number-1" @click="append(1)">1</button>
    <button style="grid-area: number-2" @click="append(2)">2</button>
    <button style="grid-area: number-3" @click="append(3)">3</button>
    <button style="grid-area: number-4" @click="append(4)">4</button>
    <button style="grid-area: number-5" @click="append(5)">5</button>
    <button style="grid-area: number-6" @click="append(6)">6</button>
    <button style="grid-area: number-7" @click="append(7)">7</button>
    <button style="grid-area: number-8" @click="append(8)">8</button>
    <button style="grid-area: number-9" @click="append(9)">9</button>
    <button style="grid-area: number-0" @click="append(0)">0</button>
    <button style="grid-area: dot" @click="append('.')">.</button>
  </div>
</template>

<script>
export default {
  name: 'vCalc',
  props: {
    btnWidth: {
      type: [String, Number],
      default: 60
    },
    btnHeight: {
      type: [String, Number],
      default: 60
    },
    fontSize: {
      type: [String, Number],
      default: 18
    }
  },
  data() {
    return {
      equation: '0',
      isDecimalAdded: false,
      isOperatorAdded: false,
      isZero: true,
      isResultShow: false
    }
  },
  computed: {
    width({ btnWidth }) {
      return /px$/.test(btnWidth) ? btnWidth : btnWidth + 'px'
    },
    height({ btnHeight }) {
      return /px$/.test(btnHeight) ? btnHeight : btnHeight + 'px'
    },
    font({ fontSize }) {
      return /px$/.test(fontSize) ? fontSize : fontSize + 'px'
    }
  },
  methods: {
    // press Operators + - × ÷
    operate(character) {
      this.isResultShow && (this.isResultShow = false)
      if (!this.isOperatorAdded) {
        this.equation += '' + character
        this.isDecimalAdded = false
        this.isOperatorAdded = true
      }
    },
    // press Numbers
    append(character) {
      this.isResultShow && this.clear()
      if (this.equation === '0') {
        if (character === '.') {
          this.equation += '' + character
          this.isDecimalAdded = true
        } else {
          this.equation = '' + character
        }
        this.isZero = false
        return
      }
      if (character === '.' && this.isDecimalAdded) return
      if (character === '.') {
        this.isDecimalAdded = true
        this.isOperatorAdded = true
      } else {
        this.isOperatorAdded = false
      }
      this.equation += '' + character
    },
    // press =
    calculate() {
      let result = this.equation
        .replace(new RegExp('×', 'g'), '*')
        .replace(new RegExp('÷', 'g'), '/')
      try {
        this.equation = parseFloat(eval(result).toFixed(9)).toString()
      } catch {
        return
      }
      this.isResultShow = true
      this.isDecimalAdded = false
      this.isOperatorAdded = false
    },
    // pressed +/-
    calculateToggle() {
      if (this.isOperatorAdded || this.isZero) return
      this.equation = this.equation + '* -1'
      this.calculate()
    },
    // pressed %
    calculatePercentage() {
      if (this.isOperatorAdded || this.isZero) return
      this.equation = this.equation + '* 0.01'
      this.calculate()
    },
    // pressed AC
    clear() {
      this.equation = '0'
      this.isDecimalAdded = false
      this.isOperatorAdded = false
      this.isZero = true
      this.isResultShow = false
    }
  }
}
</script>
<style>
body {
  background: linear-gradient(90deg, #7f7fd5 0%, #91eae4 100%);
  color: #212529;
  height: 100vh;
  display: grid;
  place-items: center;
}
</style>
<style scoped>
* {
  box-sizing: border-box;
}

.calculator {
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  background-color: rgba(221, 221, 221, 0.5);
  color: #212529;
  width: calc(var(--button-width) * 4 + var(--font) * 2);
  height: calc(var(--button-height) * 6 + var(--font) * 2);
  display: grid;
  grid-template-areas: 'result result result result' 'ac plus-minus percent divide' 'number-7 number-8 number-9 multiply' 'number-4 number-5 number-6 subtract' 'number-1 number-2 number-3 add' 'number-0 number-0 dot equal';
  grid-template-columns: repeat(4, var(--button-width));
  grid-template-rows: repeat(6, var(--button-height));
  box-shadow:
    -8px -8px 16px -10px rgba(240, 240, 246),
    8px 8px 16px -10px rgba(23, 23, 31, 0.15);
  padding: var(--font);
  border-radius: 20px;
}

.calculator button {
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  background-color: #f8f9fa;
  background: linear-gradient(135deg, #eee7 0%, #f8f9fa77 100%);
  color: #212529;
  font-family: Arial, sans-serif;
  font-size: var(--font);
  margin: 8px;
  padding: 0;
  border: 0;
  display: block;
  outline: none;
  border-radius: calc(var(--button-height) / 2);
  box-shadow:
    -4px -4px 10px -8px rgba(240, 240, 246),
    4px 4px 10px -8px rgba(23, 23, 31, 0.3);
}

.calculator button:active {
  box-shadow:
    -4px -4px 10px -8px rgba(240, 240, 246) inset,
    4px 4px 10px -8px rgba(23, 23, 31, 0.3) inset;
}

.result {
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  background-color: #ddd8;
  color: #17171f;
  font-family: Arial, sans-serif;
  font-size: calc(var(--font) * 1.5);
  line-height: var(--button-height);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: right;
  padding: 0 20px;
  border-radius: 10px;
}
</style>
