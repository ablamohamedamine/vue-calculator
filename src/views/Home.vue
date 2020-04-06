<template>
    <section>
        <template>
            <form>
                <input :value="result" readonly>
            </form>
        </template>
        <template>
            <div :key="i" v-for="i in 5">
                <template v-for="j in 4">
                    <btn-number
                    v-if="lastRange(i, j)"
                    :numberOrChar="numberAndCharacter(i, j)"
                    :key="j"
                    :class="classRedBtn(i, j)"
                    v-on:click.native="operation(i, j)"></btn-number>
                </template>
            </div>
        </template>
    </section>
</template>

<script>

import BtnNumber from "./../components/BtnNumber";

export default {
    data() {
        return {
            result: 0,
            dispalyedValue: '',
            resultToCalculat: [],
            operationSelected: false,
            virgulChar: '',
            secondClick: false,
            decimalClick: false,
            endOfcalc: true,
            // changeOperator: true,
            oldOperator: '',
            j: 0,
            i: false,
            k: false,
            operator: '',
            numberOrChar: [
                [1, 2, 3 , "+"],
                [4, 5, 6 , "-"],
                [7, 8, 9 , "*"],
                ["C", 0, "." , "/"],
                ["CLEAR", "="],
            ]
        }
    },
    name: "Home",
    components: {
        BtnNumber
    },
    computed: {
        classRedBtn() {
            return (i, j) => i == 5 ? (j == 1? "red-btn": "equal-btn") : (i == 4 && j == 1 ? "red-btn" : '')
        },
        numberAndCharacter() {
            return (i, j) => this.numberOrChar[i-1][j-1]
        }
    },
    methods: {
        lastRange(i, j) {
            if (i == 5) {

                if (j == 3 || j ==4) {

                    return false
                } else {

                    return true
                }
            }
            return true
        },
        operation(i, j) {

            var numberOrChar = this.numberAndCharacter(i, j)

            if (typeof numberOrChar == 'number') {

                if ((this.operationSelected && !this.decimalClick) || this.endOfCalc) {
                    this.dispalyedValue = ''
                }

                this.dispalyedValue += numberOrChar.toString()

                if (this.decimalClick) {

                  this.resultToCalculat[this.j-1] = this.dispalyedValue
                } else {

                    this.resultToCalculat[this.j] = this.dispalyedValue
                    this.j++
                }

                this.operationSelected = false
                this.result = this.dispalyedValue
                this.i = false
                this.k = false

            } else if (numberOrChar == '.') {

                if (!this.decimalClick) {

                  if (this.dispalyedValue == '') {

                    this.dispalyedValue = '0' + numberOrChar
                  } else {

                    this.dispalyedValue += numberOrChar
                  }
                } else if(this.operationSelected){

                    this.dispalyedValue = '0' + numberOrChar
                }

                this.result = this.dispalyedValue
                this.decimalClick = true
                this.i = false
                this.k = false

            } else if (numberOrChar == '+'
                      || numberOrChar =='-'
                      || numberOrChar == '*'
                      || numberOrChar == '/') {

                if (!this.k) {

                  if(this.operator == numberOrChar) {

                      this.oldOperator = numberOrChar
                  } else {

                      this.oldOperator = this.operator
                      if (!this.oldOperator) {

                          this.oldOperator = numberOrChar
                      }
                  }

                  this.operator = numberOrChar
                  this.operationSelected = true
                  this.decimalClick = false
                  this.dispalyedValue = ''

                  if (this.j > 1 && this.secondClick) {

                    this.result =  eval(this.resultToCalculat[this.j-2] + this.oldOperator + this.resultToCalculat[this.j-1])
                    this.resultToCalculat[this.j-1] = this.result
                  }
                  this.secondClick = true
                  this.endOfCalc = false
                  this.k = true
                  this.i = false
                }
            } else if(numberOrChar == '=' && this.resultToCalculat != '') {

                if (!this.i) {

                  this.result =  eval(this.resultToCalculat[this.j-2] + this.operator + this.resultToCalculat[this.j-1])
                  this.resultToCalculat[this.j-1] = this.result
                  this.secondClick = false
                  this.endOfCalc = true
                  this.i = true
                  this.k = false
                }

            } else if (numberOrChar == 'CLEAR') {

                this.result= 0,
                this.dispalyedValue= '',
                this.resultToCalculat= [],
                this.operationSelected= false,
                this.virgulChar= '',
                this.secondClick= false,
                this.decimalClick= false,
                this.endOfcalc= true,
                this.j= 0,
                this.operator= ''

            } else if (numberOrChar == 'C' && this.result != 0) {

              this.result = this.result.slice(0, -1)
              this.dispalyedValue = this.result
            } else {

              return
            }
        }
    }
}
</script>

<style>
@import url('../assets/style.css');
</style>
