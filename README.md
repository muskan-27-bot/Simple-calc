o# Simple-calc
This is my first repository 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div id="previous-operand"></div>
            <div id="current-operand">0</div>
        </div>
        <div class="buttons">
            <button onclick="clearDisplay()" class="span-two btn-clear">C</button>
            <button onclick="deleteNumber()">DEL</button>
            <button onclick="chooseOperator('÷')" class="btn-operator">÷</button>
            
            <button onclick="appendNumber('7')">7</button>
            <button onclick="appendNumber('8')">8</button>
            <button onclick="appendNumber('9')">9</button>
            <button onclick="chooseOperator('×')" class="btn-operator">×</button>
            
            <button onclick="appendNumber('4')">4</button>
            <button onclick="appendNumber('5')">5</button>
            <button onclick="appendNumber('6')">6</button>
            <button onclick="chooseOperator('-')" class="btn-operator">−</button>
            
            <button onclick="appendNumber('1')">1</button>
            <button onclick="appendNumber('2')">2</button>
            <button onclick="appendNumber('3')">3</button>
            <button onclick="chooseOperator('+')" class="btn-operator">+</button>
            
            <button onclick="appendNumber('.')">.</button>
            <button onclick="appendNumber('0')">0</button>
            <button onclick="compute()" class="span-two btn-equals">=</button>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
:root {
    --bg-color: #f4f7f6;
    --calc-bg: #ffffff;
    --text-color: #333;
    --accent-color: #4facfe;
    --operator-color: #ff9f0a;
    --equal-color: #2ec4b6;
}

* {
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
    background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
}

.calculator {
    width: 320px;
    background-color: var(--calc-bg);
    padding: 20px;
    border-radius: 25px;
    box-shadow: 0 20px 50px rgba(0,0,0,0.2);
}

.display {
    background-color: #222;
    color: white;
    padding: 20px;
    text-align: right;
    border-radius: 15px;
    margin-bottom: 20px;
    min-height: 100px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    word-wrap: break-word;
    word-break: break-all;
}

#previous-operand {
    color: rgba(255, 255, 255, 0.5);
    font-size: 0.9rem;
}

#current-operand {
    font-size: 2rem;
    font-weight: 500;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
}

button {
    cursor: pointer;
    font-size: 1.2rem;
    border: none;
    border-radius: 12px;
    padding: 15px 0;
    background-color: #f0f0f0;
    transition: all 0.2s ease;
    box-shadow: 0 4px 0 #ddd;
}

button:active {
    transform: translateY(2px);
    box-shadow: 0 2px 0 #ddd;
}

button:hover {
    background-color: #e5e5e5;
}

.span-two { grid-column: span 2; }

.btn-operator {
    color: var(--operator-color);
    font-weight: bold;
}

.btn-clear { color: #ff5f56; }

.btn-equals {
    background-color: var(--equal-color);
    color: white;
    box-shadow: 0 4px 0 #25a195;
}

.btn-equals:hover { background-color: #27ad9f; }
let currentInput = '0';
let previousInput = '';
let operation = null;

const currentDisplay = document.getElementById('current-operand');
const previousDisplay = document.getElementById('previous-operand');

function updateDisplay() {
    currentDisplay.innerText = currentInput;
    if (operation != null) {
        previousDisplay.innerText = `${previousInput} ${operation}`;
    } else {
        previousDisplay.innerText = '';
    }
}

function appendNumber(number) {
    if (number === '.' && currentInput.includes('.')) return;
    if (currentInput === '0' && number !== '.') {
        currentInput = number.toString();
    } else {
        currentInput = currentInput.toString() + number.toString();
    }
    updateDisplay();
}

function chooseOperator(op) {
    if (currentInput === '') return;
    if (previousInput !== '') {
        compute();
    }
    operation = op;
    previousInput = currentInput;
    currentInput = '';
    updateDisplay();
}

function compute() {
    let computation;
    const prev = parseFloat(previousInput);
    const current = parseFloat(currentInput);
    if (isNaN(prev) || isNaN(current)) return;

    switch (operation) {
        case '+': computation = prev + current; break;
        case '-': computation = prev - current; break;
        case '×': computation = prev * current; break;
        case '÷': 
            computation = current === 0 ? "Error" : prev / current; 
            break;
        default: return;
    }

    currentInput = computation.toString();
    operation = null;
    previousInput = '';
    updateDisplay();
}

function clearDisplay() {
    currentInput = '0';
    previousInput = '';
    operation = null;
    updateDisplay();
}

function deleteNumber() {
    if (currentInput.length === 1) {
        currentInput = '0';
    } else {
        currentInput = currentInput.slice(0, -1);
    }
    updateDisplay();
}
