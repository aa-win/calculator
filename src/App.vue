<template>
  <!-- Main container for calculator -->
  <div id="app" class="calculator">
    <!-- Display area for current input/result -->
    <div class="display">{{ display }}</div>
    <div class="buttons">
      <!-- Button to clear display -->
      <button class="btn clear" @click="clear">C</button>
      <!-- Parentheses buttons -->
      <button class="btn function" @click="append('(')">(</button>
      <button class="btn function" @click="append(')')">)</button>
      <!-- Division operator -->
      <button class="btn operator" @click="append('/')">/</button>

      <!-- Number buttons -->
      <button class="btn number" @click="append('7')">7</button>
      <button class="btn number" @click="append('8')">8</button>
      <button class="btn number" @click="append('9')">9</button>
      <!-- Multiplication operator -->
      <button class="btn operator" @click="append('*')">*</button>

      <button class="btn number" @click="append('4')">4</button>
      <button class="btn number" @click="append('5')">5</button>
      <button class="btn number" @click="append('6')">6</button>
      <!-- Subtraction operator -->
      <button class="btn operator" @click="append('-')">-</button>

      <button class="btn number" @click="append('1')">1</button>
      <button class="btn number" @click="append('2')">2</button>
      <button class="btn number" @click="append('3')">3</button>
      <!-- Addition operator -->
      <button class="btn operator" @click="append('+')">+</button>

      <!-- Zero and decimal point buttons -->
      <button class="btn number zero" @click="append('0')">0</button>
      <button class="btn function" @click="append('.')">.</button>
      <!-- Equals button to calculate result -->
      <button class="btn equals" @click="calculate">=</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: '', // Store current input
      display: '0', // Display result
    };
  },
  methods: {
    // Add value to current input
    append(value) {
      const lastChar = this.current[this.current.length - 1];
      // Prevent starting with invalid input
      if (
        (this.current === '' && value === '.') ||
        (this.current === '' && ['+', '-', '*', '/'].includes(value)) ||
        (['+', '-', '*', '/'].includes(lastChar) && ['+', '-', '*', '/'].includes(value))
      ) {
        return;
      }

      this.current += value;
      this.display = this.current || '0'; // Update display
    },
    // Clear input and reset display
    clear() {
      this.current = '';
      this.display = '0';
    },
    // Perform calculation and update display
    calculate() {
      try {
        // Remove last operator if needed
        if (['+', '-', '*', '/'].includes(this.current.slice(-1))) {
          this.current = this.current.slice(0, -1);
        }
        // Safely evaluate the expression
        const result = new Function(`return ${this.current}`)();
        this.display = result.toString(); // Show result
        this.current = result.toString(); // Update current input
      } catch (e) {
        this.display = 'Error'; // Show error
      }
    },
  },
};
</script>

<style scoped>
/* Styling for calculator container */
.calculator {
  width: 300px;
  margin: 50px auto;
  padding: 20px;
  border: 2px solid #ccc;
  border-radius: 8px;
  background-color: #e0e0e0;
  text-align: center;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  user-select: none;
}

/* Styling for display area */
.display {
  font-size: 2em;
  margin-bottom: 10px;
  padding: 15px;
  border: 1px solid #bbb;
  background-color: #444;
  color: white;
  text-align: right;
  border-radius: 4px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

/* Button layout */
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}

/* Button styles */
button.btn {
  font-size: 1.5em;
  padding: 15px;
  border-radius: 4px;
  cursor: pointer;
  border: 1px solid #ccc;
  transition: background-color 0.2s, transform 0.1s;
}

/* Clear button style */
button.clear {
  background-color: #e74c3c;
  color: white;
}

/* Operator buttons style */
button.operator {
  background-color: #f39c12;
  color: white;
}

/* Equals button style */
button.equals {
  background-color: #2ecc71;
  color: white;
}

/* Zero button spanning two columns */
button.zero {
  grid-column: span 2;
}

/* Number and function buttons style */
button.number {
  background-color: #3498db;
  color: white;
}

button.function {
  background-color: #95a5a6;
  color: white;
}

/* Hover effect for buttons */
button:hover {
  transform: scale(1.05);
}

button.clear:hover {
  background-color: #c0392b;
}

button.operator:hover {
  background-color: #e67e22;
}

button.equals:hover {
  background-color: #27ae60;
}

button.number:hover {
  background-color: #2980b9;
}

button.function:hover {
  background-color: #7f8c8d;
}
</style>
