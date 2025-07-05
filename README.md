# math-ai-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Math Universe</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* Tailwind gray-900 */
            color: #f3f4f6; /* Tailwind gray-100 */
        }
        .hidden {
            display: none;
        }
        .main-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .main-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 25px -5px rgba(0, 255, 255, 0.1), 0 10px 10px -5px rgba(0, 255, 255, 0.08);
        }
        .btn {
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
        }
        .btn:active {
            transform: scale(0.95);
        }

        /* --- Scoped Styles for Math Tools --- */
        #math-tools-app {
            background-color: #000;
            color: #eee;
            font-family: Arial, sans-serif;
        }
        #math-tools-app .section {
            border: 1px solid #333;
            border-radius: 10px;
            padding: 15px;
            margin: 20px auto;
            background: #111;
            max-width: 600px;
        }
        #math-tools-app h1, #math-tools-app h2 {
            color: #0ff;
            text-align: center;
        }
        #math-tools-app label {
            display: block;
            margin-top: 10px;
        }
        #math-tools-app input, #math-tools-app select {
            padding: 10px;
            margin: 5px 0;
            width: 95%;
            background: #222;
            color: #0f0;
            border: 1px solid #444;
            border-radius: 5px;
        }
        #math-tools-app button {
            padding: 10px;
            margin-top: 10px;
            width: 100%;
            background-color: #333;
            color: #0f0;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #math-tools-app button:hover {
            background-color: #555;
        }
        #math-tools-app .result {
            margin-top: 10px;
            color: #0f0;
            font-weight: bold;
        }
        #math-tools-app img {
            width: 120px;
            display: block;
            margin: 0 auto 10px;
        }

        /* --- Scoped Styles for Quiz App --- */
        #quiz-app .topic-card {
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
        }
        #quiz-app .topic-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 255, 255, 0.2);
        }
        #quiz-app .option-btn {
            border: 2px solid #4A5568;
        }
        #quiz-app .option-btn.correct {
            background-color: #2F855A;
            border-color: #38A169;
        }
        #quiz-app .option-btn.incorrect {
            background-color: #9B2C2C;
            border-color: #C53030;
        }
        #quiz-app .modal-backdrop {
            background-color: rgba(0, 0, 0, 0.75);
        }
    </style>
</head>
<body class="bg-gray-900 text-white">

    <!-- Main Container -->
    <div class="container mx-auto p-4 md:p-8 min-h-screen">

        <!-- Home Screen -->
        <div id="home-screen">
            <header class="text-center mb-12">
                <h1 class="text-5xl md:text-6xl font-extrabold text-white">
                    Welcome to <span class="text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-blue-500">AI Math Universe</span>
                </h1>
                <p class="mt-4 text-lg text-gray-300">Your one-stop destination for math tools and quizzes.</p>
            </header>
            <main class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                <!-- Card for Math Tools -->
                <div onclick="showApp('math-tools-app')" class="main-card bg-gray-800 rounded-xl p-8 cursor-pointer border border-gray-700">
                    <h2 class="text-3xl font-bold text-cyan-400 mb-4">AI Math Tools</h2>
                    <p class="text-gray-400">A collection of powerful calculators for various mathematical operations, from basic arithmetic to complex algebra and unit conversions.</p>
                </div>
                <!-- Card for Quiz App -->
                <div onclick="showApp('quiz-app')" class="main-card bg-gray-800 rounded-xl p-8 cursor-pointer border border-gray-700">
                    <h2 class="text-3xl font-bold text-green-400 mb-4">Math Quiz World</h2>
                    <p class="text-gray-400">Test your knowledge with challenging quizzes on a wide range of topics. Track your scores and compete to be the best!</p>
                </div>
            </main>
        </div>

        <!-- Math Tools App -->
        <div id="math-tools-app" class="hidden">
            <button onclick="showApp('home-screen')" class="btn bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-2 px-4 rounded-lg mb-4">← Back to Home</button>
            <img src="https://cdn-icons-png.flaticon.com/512/4712/4712100.png" alt="AI Bot" />
            <h1>🤖 AI Math Tools</h1>
            <div id="tools-container"></div>
        </div>

        <!-- Quiz App -->
        <div id="quiz-app" class="hidden">
            <!-- This div will be populated by the quiz app's HTML -->
        </div>

    </div>

    <script>
        // --- Main App Navigation ---
        const homeScreen = document.getElementById('home-screen');
        const mathToolsApp = document.getElementById('math-tools-app');
        const quizAppContainer = document.getElementById('quiz-app');

        function showApp(appId) {
            homeScreen.classList.add('hidden');
            mathToolsApp.classList.add('hidden');
            quizAppContainer.classList.add('hidden');

            document.getElementById(appId).classList.remove('hidden');
        }

        // --- Initialize Math Tools App ---
        function initMathTools() {
            const toolsContainer = document.getElementById('tools-container');
            if (toolsContainer.childElementCount > 0) return; // Already initialized

            const get = id => document.getElementById(id).value;
            
            // Handler functions for tools
            const convertTemp = () => {
                const val = get('temp');
                const [num, unit] = val.split(',');
                const temp = parseFloat(num);
                if (isNaN(temp) || !unit) return 'Invalid input';
                const upperUnit = unit.trim().toUpperCase();
                if (upperUnit === 'C') return `F: ${(temp * 9/5 + 32).toFixed(2)}, K: ${(temp + 273.15).toFixed(2)}`;
                if (upperUnit === 'F') return `C: ${((temp - 32) * 5/9).toFixed(2)}, K: ${(((temp - 32) * 5/9) + 273.15).toFixed(2)}`;
                if (upperUnit === 'K') return `C: ${(temp - 273.15).toFixed(2)}, F: ${((temp - 273.15) * 9/5 + 32).toFixed(2)}`;
                return "Enter value like 100,C";
            };
            const areaPerimeter = () => {
                const shape = get('shape').toLowerCase();
                const x = parseFloat(get('s1'));
                const y = parseFloat(get('s2'));
                if (shape === 'square') {
                    if (isNaN(x)) return 'Invalid input';
                    return `Area: ${x * x}, Perimeter: ${4 * x}`;
                }
                if (shape === 'rectangle') {
                    if (isNaN(x) || isNaN(y)) return 'Invalid input';
                    return `Area: ${x * y}, Perimeter: ${2 * (x + y)}`;
                }
                return "Use 'square' or 'rectangle'";
            };
            const unitConvert = () => {
                const value = parseFloat(get('unitval'));
                const type = get('convertType');
                if (isNaN(value)) return "❌ Error: Invalid input!";
                switch (type) {
                    case "km-cm": return `${value} km = ${value * 100000} cm`;
                    case "l-ml": return `${value} L = ${value * 1000} mL`;
                    case "cm-m": return `${value} cm = ${(value / 100).toFixed(2)} m`;
                    case "mm-cm": return `${value} mm = ${(value / 10).toFixed(2)} cm`;
                    case "l-m3": return `${value} L = ${(value / 1000).toFixed(4)} m³`;
                    default: return "❌ Invalid selection.";
                }
            };

            const tools = [
                { id: 'arithmetic', title: 'Arithmetic Calculator', label: 'Enter expression (e.g., 2+3*4):', handler: () => math.evaluate(get('arithmetic')) },
                { id: 'algebra', title: 'Algebra Simplifier', label: 'Enter algebra (e.g., 2x + 3x - x):', handler: () => math.simplify(get('algebra')).toString() },
                { id: 'birthdate', title: 'Age Calculator', label: 'Enter birthdate (YYYY-MM-DD):', type: 'date', handler: () => new Date().getFullYear() - new Date(get('birthdate')).getFullYear() + ' years' },
                { id: 'lcm', title: 'LCM Finder', label: ['value 1:', 'value 2:', 'value 3:'], ids: ['lcm1', 'lcm2', 'lcm3'], handler: () => math.lcm(+get('lcm1'), +get('lcm2'), +get('lcm3')) },
                { id: 'add', title: 'Addition', label: ['Number 1:', 'Number 2:'], ids: ['add1', 'add2'], handler: () => +get('add1') + +get('add2') },
                { id: 'sub', title: 'Subtraction', label: ['Number 1:', 'Number 2:'], ids: ['sub1', 'sub2'], handler: () => +get('sub1') - +get('sub2') },
                { id: 'exp', title: 'Exponent / Power', label: ['Base:', 'Exponent:'], ids: ['base', 'exp'], handler: () => Math.pow(+get('base'), +get('exp')) },
                { id: 'temp', title: 'Temperature Converter', label: 'Value (e.g., 100,C):', handler: convertTemp },
                { id: 'speed', title: 'Speed Calculator', label: ['Distance (km):', 'Time (hr):'], ids: ['dist', 'time'], handler: () => (+get('dist') / +get('time')).toFixed(2) + ' km/h' },
                { id: 'velocity', title: 'Velocity Calculator', label: ['Displacement (m):', 'Time (s):'], ids: ['disp', 'time2'], handler: () => (+get('disp') / +get('time2')).toFixed(2) + ' m/s' },
                { id: 'percent', title: 'Percentage Calculator', label: ['Part:', 'Whole:'], ids: ['part', 'whole'], handler: () => ((+get('part') / +get('whole')) * 100).toFixed(2) + '%' },
                { id: 'sq', title: 'Square & Cube', label: 'Enter Number:', handler: () => 'Square: ' + get('sq') ** 2 + ', Cube: ' + get('sq') ** 3 },
                { id: 'shape', title: 'Area & Perimeter', label: ['Shape (square or rectangle):', 'Value 1:', 'Value 2 (if rectangle):'], ids: ['shape', 's1', 's2'], handler: areaPerimeter },
                { id: 'unitconvert', title: 'Unit Converter', label: ['Enter Value:', 'Select Conversion:'], ids: ['unitval', 'convertType'], handler: unitConvert }
            ];
            
            // Tool creation logic
            const createTool = ({ id, title, label, handler, ids = [id], type = 'text' }) => {
                const container = document.createElement('div');
                container.className = 'section';
                const h2 = document.createElement('h2');
                h2.innerText = title;
                container.appendChild(h2);
                (Array.isArray(label) ? label : [label]).forEach((lbl, i) => {
                    const inputId = ids[i];
                    const lblElem = document.createElement('label');
                    lblElem.innerText = lbl;
                    container.appendChild(lblElem);
                    if (inputId === 'convertType') {
                        const select = document.createElement('select');
                        select.id = 'convertType';
                        const options = [ { value: 'km-cm', text: 'Kilometers to Centimeters' }, { value: 'l-ml', text: 'Litres to Millilitres' }, { value: 'cm-m', text: 'Centimeters to Meters' }, { value: 'mm-cm', text: 'Millimeters to Centimeters' }, { value: 'l-m3', text: 'Litres to Cubic Meters' }];
                        options.forEach(opt => { const o = document.createElement('option'); o.value = opt.value; o.textContent = opt.text; select.appendChild(o); });
                        container.appendChild(select);
                    } else {
                        const input = document.createElement('input');
                        input.id = inputId;
                        input.type = type;
                        container.appendChild(input);
                    }
                });
                const button = document.createElement('button');
                button.innerText = 'Calculate';
                button.onclick = () => {
                    try {
                        document.getElementById(id + '-result').innerText = 'Result: ' + handler();
                    } catch (e) {
                        document.getElementById(id + '-result').innerText = 'Error: ' + e.message;
                    }
                };
                container.appendChild(button);
                const resultDiv = document.createElement('div');
                resultDiv.className = 'result';
                resultDiv.id = id + '-result';
                container.appendChild(resultDiv);
                toolsContainer.appendChild(container);
            };
            tools.forEach(createTool);
        }

        // --- Initialize Quiz App ---
        function initQuizApp() {
            if (quizAppContainer.childElementCount > 0) return; // Already initialized

            quizAppContainer.innerHTML = `
                <div class="container mx-auto p-4 md:p-8 min-h-screen flex flex-col items-center justify-center">
                    <!-- Nickname Screen -->
                    <div id="nickname-screen" class="w-full max-w-md text-center">
                        <button onclick="showApp('home-screen')" class="btn bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-lg mb-8 absolute top-4 left-4">← Home</button>
                        <h1 class="text-4xl md:text-5xl font-bold text-cyan-400 mb-4">Math Quiz World</h1>
                        <p class="text-lg text-gray-300 mb-8">Enter your nickname to begin</p>
                        <form id="nickname-form" class="space-y-4">
                            <input type="text" id="nickname-input" placeholder="Your Nickname" class="w-full px-4 py-3 bg-gray-800 border-2 border-gray-700 rounded-lg text-white focus:outline-none focus:border-cyan-500 text-lg">
                            <button type="submit" class="btn w-full bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-3 px-4 rounded-lg text-lg">Start Playing</button>
                        </form>
                        <button id="view-scoreboard-btn-main" class="btn w-full mt-4 bg-gray-700 hover:bg-gray-600 text-white font-bold py-3 px-4 rounded-lg text-lg">View Scoreboard</button>
                    </div>
                    <!-- Topic Selection Screen -->
                    <div id="topic-screen" class="hidden w-full max-w-4xl">
                        <div class="flex justify-between items-center mb-8">
                            <div>
                                <h2 class="text-3xl font-bold">Welcome, <span id="player-name"></span>!</h2>
                                <p class="text-gray-400">Choose a topic to test your skills.</p>
                            </div>
                            <div>
                                <button id="view-scoreboard-btn" class="btn bg-gray-700 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded-lg mr-2">Scoreboard</button>
                                <button onclick="showApp('home-screen')" class="btn bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-lg">← Home</button>
                            </div>
                        </div>
                        <div id="topic-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"></div>
                    </div>
                    <!-- Quiz Screen -->
                    <div id="quiz-screen" class="hidden w-full max-w-2xl">
                        <div class="bg-gray-800 p-6 rounded-lg shadow-2xl">
                            <div class="flex justify-between items-center mb-4">
                                <h3 id="quiz-topic-title" class="text-2xl font-bold text-cyan-400">Topic</h3>
                                <div class="text-lg font-semibold">Score: <span id="score">0</span></div>
                            </div>
                            <div id="question-container" class="mb-6">
                                <p class="text-right text-gray-400 mb-2">Question <span id="question-number">1</span>/10</p>
                                <p id="question-text" class="text-xl md:text-2xl font-semibold min-h-[6rem] flex items-center"></p>
                            </div>
                            <div id="options-container" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
                            <div class="mt-6 text-right">
                                <button id="next-question-btn" class="btn bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-2 px-6 rounded-lg hidden">Next</button>
                                <button id="quit-quiz-btn" class="btn bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-6 rounded-lg">Quit</button>
                            </div>
                        </div>
                    </div>
                    <!-- Results Screen -->
                    <div id="results-screen" class="hidden w-full max-w-md text-center">
                        <div class="bg-gray-800 p-8 rounded-lg shadow-2xl">
                            <h2 class="text-3xl font-bold mb-2">Quiz Complete!</h2>
                            <p class="text-cyan-400 text-lg mb-4">Well done, <span id="result-player-name"></span>!</p>
                            <p class="text-xl mb-1">Your score for <strong id="result-topic"></strong> (<span id="result-level"></span>):</p>
                            <p class="text-6xl font-bold my-4"><span id="final-score"></span> / 10</p>
                            <p id="result-message" class="text-gray-300 mb-6"></p>
                            <button id="play-again-btn" class="btn w-full bg-cyan-600 hover:bg-cyan-700 text-white font-bold py-3 px-4 rounded-lg text-lg">Play Another Topic</button>
                        </div>
                    </div>
                    <!-- Scoreboard Screen -->
                    <div id="scoreboard-screen" class="hidden w-full max-w-lg">
                        <div class="bg-gray-800 p-8 rounded-lg shadow-2xl">
                            <h2 class="text-3xl font-bold text-center mb-6 text-cyan-400">Scoreboard</h2>
                            <div id="scoreboard-list" class="space-y-3 max-h-96 overflow-y-auto"></div>
                            <div class="mt-6 flex justify-center gap-4">
                                <button id="back-to-main-btn" class="btn bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-6 rounded-lg">Back</button>
                                <button id="clear-scores-btn" class="btn bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-6 rounded-lg">Clear Scores</button>
                            </div>
                        </div>
                    </div>
                    <!-- Level Selection Modal -->
                    <div id="level-modal" class="fixed inset-0 modal-backdrop items-center justify-center hidden">
                        <div class="bg-gray-800 rounded-lg shadow-2xl p-8 w-full max-w-sm text-center">
                            <h3 class="text-2xl font-bold mb-4">Select Difficulty</h3>
                            <p class="text-gray-400 mb-6">Choose a level for <strong id="modal-topic-name"></strong>.</p>
                            <div class="space-y-4">
                                <button data-level="Standard" class="level-btn btn w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg text-lg">Standard</button>
                                <button data-level="Up-level" class="level-btn btn w-full bg-yellow-600 hover:bg-yellow-700 text-white font-bold py-3 rounded-lg text-lg">Up-level</button>
                            </div>
                            <button id="close-modal-btn" class="mt-6 text-gray-400 hover:text-white">Cancel</button>
                        </div>
                    </div>
                </div>
            `;
            
            // --- Quiz App Logic ---
            const quizScreens = {
                nickname: document.getElementById('nickname-screen'),
                topic: document.getElementById('topic-screen'),
                quiz: document.getElementById('quiz-screen'),
                results: document.getElementById('results-screen'),
                scoreboard: document.getElementById('scoreboard-screen'),
            };
            const nicknameForm = document.getElementById('nickname-form');
            const nicknameInput = document.getElementById('nickname-input');
            const playerNameSpan = document.getElementById('player-name');
            const topicGrid = document.getElementById('topic-grid');
            const quizTopicTitle = document.getElementById('quiz-topic-title');
            const scoreSpan = document.getElementById('score');
            const questionNumberSpan = document.getElementById('question-number');
            const questionText = document.getElementById('question-text');
            const optionsContainer = document.getElementById('options-container');
            const nextQuestionBtn = document.getElementById('next-question-btn');
            const quitQuizBtn = document.getElementById('quit-quiz-btn');
            const resultPlayerName = document.getElementById('result-player-name');
            const resultTopic = document.getElementById('result-topic');
            const resultLevel = document.getElementById('result-level');
            const finalScoreSpan = document.getElementById('final-score');
            const resultMessage = document.getElementById('result-message');
            const playAgainBtn = document.getElementById('play-again-btn');
            const levelModal = document.getElementById('level-modal');
            const modalTopicName = document.getElementById('modal-topic-name');
            const closeModalBtn = document.getElementById('close-modal-btn');
            const scoreboardList = document.getElementById('scoreboard-list');
            const viewScoreboardBtn = document.getElementById('view-scoreboard-btn');
            const viewScoreboardBtnMain = document.getElementById('view-scoreboard-btn-main');
            const backToMainBtn = document.getElementById('back-to-main-btn');
            const clearScoresBtn = document.getElementById('clear-scores-btn');

            let state = {
                nickname: '',
                currentTopic: '',
                currentLevel: '',
                currentQuestions: [],
                questionIndex: 0,
                score: 0,
                scores: JSON.parse(localStorage.getItem('mathQuizScores')) || [],
            };

            const quizData = {
                "Addition": { generate: generateArithmeticQuestions('+') }, "Subtraction": { generate: generateArithmeticQuestions('-') }, "Multiplication": { generate: generateArithmeticQuestions('*', 2, 12, 2, 25) }, "Division": { generate: generateDivisionQuestions }, "Percentage": { generate: generatePercentageQuestions }, "Exponents & Powers": { generate: generateExponentQuestions }, "Square & Cube Roots": { generate: generateRootQuestions }, "Area of Rectangle": { generate: generateAreaQuestions }, "LCM": { generate: generateLCMQuestions }, "Algebraic Expressions": { generate: generateAlgebraQuestions }, "Speed, Distance, Time": { generate: generateSpeedQuestions }, "Temperature Conversion": { generate: generateTempQuestions },
            };

            function generateArithmeticQuestions(operator, s1 = 20, e1 = 200, s2 = 20, e2 = 200) { return (level) => { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const num1 = rand(isHard ? s1 : 1, isHard ? e1 : 50); const num2 = rand(isHard ? s2 : 1, isHard ? e2 : 50); let question, answer; switch (operator) { case '+': question = `What is ${num1} + ${num2}?`; answer = num1 + num2; break; case '-': const [n1, n2] = (num1 > num2) ? [num1, num2] : [num2, num1]; question = `What is ${n1} - ${n2}?`; answer = n1 - n2; break; case '*': question = `What is ${num1} * ${num2}?`; answer = num1 * num2; break; } questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }; }
            function generateDivisionQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const divisor = rand(isHard ? 5 : 2, isHard ? 20 : 10); const quotient = rand(isHard ? 10 : 2, isHard ? 50 : 20); const dividend = divisor * quotient; const question = `What is ${dividend} ÷ ${divisor}?`; const answer = quotient; questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function generatePercentageQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const percentage = rand(1, 9) * 10; const number = rand(isHard ? 100 : 10, isHard ? 1000 : 200); const question = `What is ${percentage}% of ${number}?`; const answer = (percentage / 100) * number; questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function generateExponentQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const base = rand(isHard ? 4 : 2, isHard ? 10 : 5); const exponent = rand(isHard ? 3 : 2, isHard ? 4 : 3); const question = `What is ${base} ^ ${exponent}?`; const answer = Math.pow(base, exponent); questions.push({ question, answer: answer.toString(), options: generateOptions(answer, answer * 2) }); } return questions; }
            function generateRootQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { let question, answer; if (i % 2 === 0 || !isHard) { const root = rand(isHard ? 10 : 2, isHard ? 25 : 12); const num = root * root; question = `What is the square root of ${num}?`; answer = root; } else { const root = rand(3, 7); const num = root * root * root; question = `What is the cube root of ${num}?`; answer = root; } questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function generateAreaQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const length = rand(isHard ? 10 : 2, isHard ? 50 : 20); const width = rand(isHard ? 10 : 2, isHard ? 50 : 20); const area = length * width; const perimeter = 2 * (length + width); const question = `A rectangle has a length of ${length} and a width of ${width}. What is its area?`; const options = generateOptions(area, perimeter); questions.push({ question, answer: area.toString(), options }); } return questions; }
            function gcd(a, b) { return b === 0 ? a : gcd(b, a % b); }
            function lcm(a, b) { return (a * b) / gcd(a, b); }
            function generateLCMQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const num1 = rand(isHard ? 5 : 2, isHard ? 20 : 10); const num2 = rand(isHard ? 5 : 2, isHard ? 20 : 10); const answer = lcm(num1, num2); const question = `What is the LCM of ${num1} and ${num2}?`; const options = generateOptions(answer, num1 * num2); questions.push({ question, answer: answer.toString(), options }); } return questions; }
            function generateAlgebraQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { let question, answer; if (!isHard || i % 2 === 0) { const a = rand(2, 10); const b = rand(2, 10); const c = rand(2, 10); question = `If a=${a} and b=${b}, what is the value of ${c}a + ${b}b?`; answer = c * a + b * b; } else { const coeff = rand(2, 5); const constant = rand(5, 20); const result = rand(25, 50); question = `Solve for x: ${coeff}x + ${constant} = ${result + constant}`; answer = result / coeff; } questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function generateSpeedQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { const time = rand(2, 5); const speed = rand(isHard ? 80 : 40, isHard ? 150 : 70); const distance = speed * time; const qType = rand(1, 3); let question, answer; if(qType === 1) { question = `A car travels ${distance} km in ${time} hours. What is its average speed in km/h?`; answer = speed; } else if (qType === 2) { question = `A train moves at a speed of ${speed} km/h for ${time} hours. How far does it travel?`; answer = distance; } else { question = `How long does it take to travel ${distance} km at a speed of ${speed} km/h?`; answer = time; } questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function generateTempQuestions(level) { const questions = []; const isHard = level === 'Up-level'; for (let i = 0; i < 10; i++) { let question, answer; const tempC = rand(isHard ? -10 : 0, isHard ? 40 : 30) * 5; const tempF = Math.round(tempC * 9/5 + 32); if (i % 2 === 0) { question = `Convert ${tempC}°C to Fahrenheit.`; answer = tempF; } else { question = `Convert ${tempF}°F to Celsius.`; answer = tempC; } questions.push({ question, answer: answer.toString(), options: generateOptions(answer) }); } return questions; }
            function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
            function shuffle(array) { for (let i = array.length - 1; i > 0; i--) { const j = Math.floor(Math.random() * (i + 1));[array[i], array[j]] = [array[j], array[i]]; } return array; }
            function generateOptions(correctAnswer, specificWrongAnswer) { const options = new Set([correctAnswer.toString()]); if (specificWrongAnswer !== undefined && specificWrongAnswer !== correctAnswer) { options.add(specificWrongAnswer.toString()); } while (options.size < 4) { const offset = rand(1, 10); const wrong = Math.max(0, correctAnswer + (Math.random() > 0.5 ? offset : -offset)); if(wrong !== correctAnswer) { options.add(wrong.toString()); } } return shuffle(Array.from(options)); }
            
            function showQuizScreen(screenName) { Object.values(quizScreens).forEach(screen => screen.classList.add('hidden')); if (quizScreens[screenName]) { quizScreens[screenName].classList.remove('hidden'); } }
            function handleNicknameSubmit(e) { e.preventDefault(); const nickname = nicknameInput.value.trim(); if (nickname) { state.nickname = nickname; playerNameSpan.textContent = nickname; resultPlayerName.textContent = nickname; showQuizScreen('topic'); } }
            function populateTopics() { topicGrid.innerHTML = ''; for (const topic in quizData) { const card = document.createElement('div'); card.className = 'topic-card bg-gray-800 p-4 rounded-lg text-center cursor-pointer flex flex-col justify-center items-center h-32'; card.dataset.topic = topic; card.innerHTML = `<h3 class="text-lg font-semibold">${topic}</h3>`; card.addEventListener('click', () => selectTopic(topic)); topicGrid.appendChild(card); } }
            function selectTopic(topic) { state.currentTopic = topic; modalTopicName.textContent = topic; levelModal.classList.remove('hidden'); levelModal.classList.add('flex'); }
            function startQuiz() { state.questionIndex = 0; state.score = 0; state.currentQuestions = shuffle(quizData[state.currentTopic].generate(state.currentLevel)); quizTopicTitle.textContent = `${state.currentTopic} (${state.currentLevel})`; showQuizScreen('quiz'); displayQuestion(); }
            function displayQuestion() { if (state.questionIndex >= state.currentQuestions.length) { endQuiz(); return; } const q = state.currentQuestions[state.questionIndex]; questionText.textContent = q.question; scoreSpan.textContent = state.score; questionNumberSpan.textContent = state.questionIndex + 1; optionsContainer.innerHTML = ''; nextQuestionBtn.classList.add('hidden'); q.options.forEach(option => { const button = document.createElement('button'); button.className = 'option-btn btn p-4 rounded-lg text-left text-lg'; button.textContent = option; button.addEventListener('click', () => selectAnswer(button, q.answer)); optionsContainer.appendChild(button); }); }
            function selectAnswer(button, correctAnswer) { const isCorrect = button.textContent === correctAnswer; Array.from(optionsContainer.children).forEach(btn => { btn.disabled = true; if (btn.textContent === correctAnswer) { btn.classList.add('correct'); } }); if (isCorrect) { state.score++; button.classList.add('correct'); } else { button.classList.add('incorrect'); } scoreSpan.textContent = state.score; nextQuestionBtn.classList.remove('hidden'); nextQuestionBtn.onclick = () => { state.questionIndex++; displayQuestion(); }; }
            function endQuiz() { finalScoreSpan.textContent = state.score; resultTopic.textContent = state.currentTopic; resultLevel.textContent = state.currentLevel; const percentage = (state.score / 10) * 100; if (percentage === 100) { resultMessage.textContent = "Perfect score! You're a math genius!"; } else if (percentage >= 70) { resultMessage.textContent = "Great job! You really know your stuff."; } else if (percentage >= 50) { resultMessage.textContent = "Good effort! A little more practice and you'll be an expert."; } else { resultMessage.textContent = "Keep practicing! Every attempt makes you better."; } saveScore(); showQuizScreen('results'); }
            function saveScore() { const newScore = { name: state.nickname, topic: state.currentTopic, level: state.currentLevel, score: state.score, date: new Date().toISOString() }; state.scores.push(newScore); state.scores.sort((a, b) => b.score - a.score || new Date(b.date) - new Date(a.date)); state.scores = state.scores.slice(0, 100); localStorage.setItem('mathQuizScores', JSON.stringify(state.scores)); }
            function showScoreboard() { scoreboardList.innerHTML = ''; if (state.scores.length === 0) { scoreboardList.innerHTML = '<p class="text-center text-gray-400">No scores recorded yet. Play a game to see your name here!</p>'; } else { state.scores.forEach((s, index) => { const entry = document.createElement('div'); entry.className = 'flex justify-between items-center bg-gray-700 p-3 rounded-md'; entry.innerHTML = `<div class="flex items-center"><span class="font-bold text-lg w-8">${index + 1}.</span><div><p class="font-semibold">${s.name}</p><p class="text-sm text-gray-400">${s.topic} (${s.level})</p></div></div><span class="font-bold text-xl text-cyan-400">${s.score} / 10</span>`; scoreboardList.appendChild(entry); }); } showQuizScreen('scoreboard'); }
            function clearScores() { if (confirm("Are you sure you want to clear all scores? This action cannot be undone.")) { state.scores = []; localStorage.removeItem('mathQuizScores'); showScoreboard(); } }
            
            // Initial setup for quiz event listeners
            populateTopics();
            nicknameForm.addEventListener('submit', handleNicknameSubmit);
            playAgainBtn.addEventListener('click', () => showQuizScreen('topic'));
            quitQuizBtn.addEventListener('click', () => { if(confirm("Are you sure you want to quit? Your progress will be lost.")) { showQuizScreen('topic'); } });
            closeModalBtn.addEventListener('click', () => levelModal.classList.add('hidden'));
            levelModal.addEventListener('click', (e) => { if (e.target === levelModal) { levelModal.classList.add('hidden'); } });
            document.querySelectorAll('.level-btn').forEach(btn => { btn.addEventListener('click', () => { state.currentLevel = btn.dataset.level; levelModal.classList.add('hidden'); startQuiz(); }); });
            viewScoreboardBtn.addEventListener('click', showScoreboard);
            viewScoreboardBtnMain.addEventListener('click', showScoreboard);
            backToMainBtn.addEventListener('click', () => { showQuizScreen(state.nickname ? 'topic' : 'nickname'); });
            clearScoresBtn.addEventListener('click', clearScores);
            
            showQuizScreen('nickname');
        }

        // --- App Entry Point ---
        document.addEventListener('DOMContentLoaded', () => {
            initMathTools();
            initQuizApp();
            showApp('home-screen'); // Start at the home screen
        });

    </script>
</body>
</html>
