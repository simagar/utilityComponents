<template>
  <div class="flex items-center justify-between gap-2 my-2 " dir="ltr">
    <div class="p-3 py-1 bg-white rounded w-full flex justify-between ">
      <input v-for="i in optCount" :ref="'Input'+i"
             class="w-2 h-full bg-transparent focus:outline-none" maxlength="1"
             placeholder="-"
             type="tel"
             @input="NextElement" @keydown.backspace="PrevElement">
    </div>

  </div>

</template>
<script>

export default {
  name: 'OtpInput',
  data() {
    return {
      counter: 1,
      Result: [],
      numberIndicators: {
        1: 'five',
        2: 'six',
        3: 'seven',
        4: 'eight',
        5: 'nine',
        6: 'ten',
        7: 'eleven',
      }
    }
  },
  props: {
    optCount: {
      type: Number,
      default: 7
    },
    five: {
      type: Number,
    },
    six: {
      type: Number,
    },
    seven: {
      type: Number,
    },
    eight: {
      type: Number,
    },
    nine: {
      type: Number,
    },
    ten: {
      type: Number,
    },
    eleven: {
      type: Number,
    },
  },
  watch: {
    five: {
      handler(val) {
        if (val) {
          // Object.keys(this.numberIndicators).forEach((key) => {
          //   console.log(this.$refs[`Input${key}`])
          //   this.$refs[`Input${key}`][0].value = this[this.numberIndicators[key]]
          // })
        }
      },
      immediate: true
    }
  },
  methods: {
    getValueByIndicator(indicator) {
      if (this[this.numberIndicators[indicator]]) {
        return this[this.numberIndicators[indicator]];
      }
    },
    NextElement() {
      this.counter++
      if (this.$refs[`Input${this.counter}`] && this.$refs[`Input${this.counter}`][0]) {
        this.$refs[`Input${this.counter}`][0].focus()
      }
      else {
        for (let i = 1; i < this.counter; i++) {
          this.Result.push(this.$refs[`Input${i}`][0].value)
        }
        let NewResult = this.Result.join('')
        this.$emit('getNumber', NewResult)

      }

    },
    PrevElement() {
      this.counter--
      if (this.$refs[`Input${this.counter}`] && this.$refs[`Input${this.counter}`][0]) {
        this.$emit('getNumber', [])
        this.Result = []
        this.$refs[`Input${this.counter}`][0].focus()
      }
    }
  }

};
</script>

<style scoped>
.single-input {
  width: 1rem;
  border-bottom: 1px solid black;
  margin: 0 0.3rem;
  background-color: transparent;
}

.edit-number-input {
  border: 1px solid rgb(0, 174, 255);
}

.otp-input::-webkit-input-placeholder {
  text-align: right;
}

.otp-input:-moz-placeholder {
  text-align: right;
}

.submit-button:disabled {
  cursor: initial;
}

.submit-button {
  transition: all 0.1s;
  width: 40%;
}

.single-input,
.single-input::-webkit-outer-spin-button,
.single-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  -moz-appearance: textfield;
}
</style>
