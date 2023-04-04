<template>
  <div class="calculator-bg" :class="'theme-' + theme">
    <div class="calc-container">
      <header-component @toggle-theme="theme = theme < 3 ? theme + 1 : 1" />
      <div class="screen">
          <input class="result-text" type="text" :value="screen" disabled>
      </div>
      
      <div class="controls">
        <action-button v-for="(button) in controls" :key="button.control" class="button-text" :class="button.type + ' ' + button.control" @click="takeInput(button.control)">
          {{ button.control }}
        </action-button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import HeaderComponent from './components/HeaderComponent.vue';
import ActionButton from './components/shared/ActionButton.vue';
import { create, all } from 'mathjs'

const config = { };
const math = create(all, config);

export default defineComponent({
  components: {
    ActionButton,
    HeaderComponent,
  },
  data() {
    return {
      theme: 2,
      controls: [
        {
          control: 7,
          type: "primary",
        },
        {
          control: 8,
          type: "primary",
        },
        {
          control: 9,
          type: "primary",
        },
        {
          control: "DEL",
          type: "secondary",
        },
        {
          control: 4,
          type: "primary",
        },
        {
          control: 5,
          type: "primary",
        },
        {
          control: 6,
          type: "primary",
        },
        {
          control: "+",
          type: "primary",
        },
        {
          control: 1,
          type: "primary",
        },
        {
          control: 2,
          type: "primary",
        },
        {
          control: 3,
          type: "primary",
        },
        {
          control: "-",
          type: "primary",
        },
        {
          control: ".",
          type: "primary",
        },
        {
          control: 0,
          type: "primary",
        },
        {
          control: "/",
          type: "primary",
        },
        {
          control: "x",
          type: "primary",
        },
        {
          control: "RESET",
          type: "secondary",
        },
        {
          control: "=",
          type: "danger",
        },
      ],
      buffer: "",
      operation: "",
      firstNumber: "",
      secondNumber: "",
    }
  },
  methods: {
    takeInput (input: string | number) {
      if (typeof input == "number" || input == ".") {
        this.buffer = this.buffer + input;
      } else {
        switch (input) {
          case "DEL":
            this.buffer = this.buffer.slice(0, this.buffer.length - 1);
            break;
          case "RESET": 
            this.reset();
            break;
          case "=":
            this.calculate();
            break;
          default: 
            this.setOperation(input);
            break;
        }
      }
    },

    reset() {
      this.buffer = "";
      this.firstNumber = "";
      this.secondNumber = "";
      this.operation = "";
    },

    calculate() {
      if (this.buffer) {
        this.secondNumber = this.buffer;
      } else {
        this.secondNumber = this.firstNumber;
      }
      const first = Number.parseFloat(this.firstNumber);
      const second = Number.parseFloat(this.secondNumber);
      const operations= {
        "x": "multiply",
        "/": "divide",
        "-": "subtract",
        "+": "add",
      }
      const operationString = operations[this.operation];
      const result = math[operationString](first,second);
      this.reset();
      this.buffer = result.toString();
    },

    setOperation(input: string) {
      if (this.operation && this.firstNumber) {
        this.calculate();
        return this.setOperation(input);
      }
      this.firstNumber = this.buffer;
      this.buffer = "";
      if (this.firstNumber) {
        this.operation = input
      }
    }
  },
  computed: {
    screen () {
      if (this.buffer == "") {
        return "0";
      } else {
        return this.buffer;
      }
    }
  }
})
</script>

<style>
@reset-global pc;
@reset-global mobile;

.calculator-bg {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  padding: 30px 24px;
}

.calc-container {
  width: 100%;
  max-width: 540px;;
}

.screen {
  height: 88px;
  width: 100%;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: end;
  padding-left: 24px;
  padding-right: 24px;
  margin-top: 32px;
}

.result-text {
  font-size: 40px;
  width: 100%;

  &input {
    background: none;
    outline: none;
    border: none;
    text-align :right;
  }
}

.controls {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 13px;
  padding: 24px;
  border-radius: 10px;
  margin-top: 24px;;
}

.button-text {
  font-size: 32px;
}

.secondary {
  font-size: 20px;
}

.secondary:not(.DEL), .danger {
  grid-column: span 2;
}
</style>
