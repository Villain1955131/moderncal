<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Calculator</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #fff;
        }

        .calculator {
            width: 320px;
            max-width: 90vw;
            background: linear-gradient(145deg, #2c3e50, #34495e);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3), inset 0 1px 0 rgba(255, 255, 255, 0.1);
            position: relative;
            overflow: hidden;
        }

        .calculator::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, #f39c12, #e74c3c, #27ae60);
            border-radius: 20px 20px 0 0;
        }

        #display {
            width: 100%;
            height: 60px;
            font-size: 28px;
            text-align: right;
            margin-bottom: 25px;
            padding: 15px;
            border: none;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.2);
            backdrop-filter: blur(10px);
            outline: none;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
        }

        button {
            height: 60px;
            font-size: 20px;
            font-weight: 500;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            background: linear-gradient(145deg, #7f8c8d, #95a5a6);
            color: #fff;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2), inset 0 1px 0 rgba(255, 255, 255, 0.1);
            transition: all 0.2s ease;
            position: relative;
            overflow: hidden;
        }

        button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s;
        }

        button:hover::before {
            left: 100%;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
        }

        button:active {
            transform: translateY(0);
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        }

        .operator {
            background: linear-gradient(145deg, #f39c12, #e67e22);
        }

        .operator:hover {
            background: linear-gradient(145deg, #e67e22, #d35400);
        }

        .equals {
            background: linear-gradient(145deg, #27ae60, #229954);
            grid-column: span 2;
        }

        .equals:hover {
            background: linear-gradient(145deg, #229954, #1e8449);
        }

        .clear {
            background: linear-gradient(145deg, #e74c3c, #c0392b);
        }

        .clear:hover {
            background: linear-gradient(145deg, #c0392b, #a93226);
        }

        .zero {
            grid-column: span 2;
        }

        @media (max-width: 480px) {
            .calculator {
                width: 280px;
                padding: 20px;
            }
            #display {
                height: 50px;
                font-size: 24px;
            }
            button {
                height: 50px;
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" id="display" readonly>
        <div class="buttons">
            <button class="clear">C</button>
            <button class="operator">/</button>
            <button class="operator">*</button>
            <button class="operator">-</button>
            <button>7</button>
            <button>8</button>
            <button>9</button>
            <button class="operator">+</button>
            <button>4</button>
            <button>5</button>
            <button>6</button>
            <button class="equals">=</button>
            <button>1</button>
            <button>2</button>
            <button>3</button>
            <button class="zero">0</button>
            <button>.</button>
        </div>
    </div>
    <script>
        const display = document.getElementById('display');
        let currentInput = '';
        let operator = '';
        let previousInput = '';

        document.querySelectorAll('button').forEach(button => {
            button.addEventListener('click', () => {
                const value = button.textContent;

                if (value === 'C') {
                    currentInput = '';
                    operator = '';
                    previousInput = '';
                    display.value = '';
                } else if (value === '=') {
                    if (previousInput && operator && currentInput) {
                        try {
                            display.value = eval(`${previousInput} ${operator} ${currentInput}`);
                            currentInput = display.value;
                            operator = '';
                            previousInput = '';
                        } catch {
                            display.value = 'Error';
                            currentInput = '';
                        }
                    }
                } else if (['+', '-', '*', '/'].includes(value)) {
                    if (currentInput) {
                        if (previousInput && operator) {
                            try {
                                previousInput = eval(`${previousInput} ${operator} ${currentInput}`);
                                display.value = previousInput;
                            } catch {
                                display.value = 'Error';
                                previousInput = '';
                            }
                        } else {
                            previousInput = currentInput;
                        }
                        operator = value;
                        currentInput = '';
                    }
                } else {
                    currentInput += value;
                    display.value = currentInput;
                }
            });
        });
    </script>
</body>
</html>
