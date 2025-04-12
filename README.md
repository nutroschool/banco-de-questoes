<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plataforma de Estudos - Nutrologia</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Simple transition */
        .fade-in { animation: fadeIn 0.5s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; border-radius: 10px;}
        ::-webkit-scrollbar-thumb { background: #aaa; border-radius: 10px;}
        ::-webkit-scrollbar-thumb:hover { background: #888; }
        body { font-family: 'Inter', sans-serif; }
        /* Custom styles for answer feedback */
        .correct-answer-highlight { background-color: #d1fae5; border-color: #06a561; } /* Tailwind green-100/green-500 */
        .incorrect-answer-highlight { background-color: #fee2e2; border-color: #ef4444; } /* Tailwind red-100/red-500 */
        .selected-answer-border { border-width: 2px; }
    </style>
</head>
<body class="bg-gray-100 text-gray-800">

    <div id="app" class="container mx-auto p-4 max-w-4xl">

        <div id="login-screen" class="bg-white p-8 rounded-lg shadow-md fade-in">
            <h1 class="text-2xl font-bold mb-6 text-center text-blue-600">Plataforma de Estudos - Nutrologia</h1>
            <div class="mb-4">
                <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                <input type="email" id="email" value="admin@teste.com" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500" placeholder="seuemail@exemplo.com">
            </div>
            <div class="mb-6">
                <label for="password" class="block text-sm font-medium text-gray-700 mb-1">Senha</label>
                <input type="password" id="password" value="admin" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500" placeholder="********">
                <p class="text-xs text-gray-500 mt-1">Use admin@teste.com / admin OU aluno@teste.com / aluno</p>
            </div>
            <button onclick="login()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md transition duration-150 ease-in-out">Entrar</button>
            <p id="login-error" class="text-red-500 text-sm mt-4 text-center"></p>
        </div>

        <div id="main-content" class="hidden fade-in">
            <header class="bg-white p-4 rounded-lg shadow-md mb-6 flex justify-between items-center">
                <h1 class="text-xl font-bold text-blue-600">Plataforma Nutrologia</h1>
                <div>
                    <span id="user-info" class="mr-4 text-sm text-gray-600"></span>
                    <button onclick="logout()" class="bg-red-500 hover:bg-red-600 text-white text-sm font-semibold py-1 px-3 rounded-md transition duration-150 ease-in-out">Sair</button>
                </div>
            </header>

            <div id="admin-view" class="hidden bg-white p-6 rounded-lg shadow-md mb-6 space-y-6">
                <h2 class="text-lg font-semibold border-b pb-2">Área Administrativa</h2>

                <div>
                    <h3 class="font-medium mb-2 text-base">Gerenciar Temas</h3>
                    <div class="flex gap-2 mb-3">
                        <input type="text" id="new-topic-name" placeholder="Nome do novo tema" class="flex-grow px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500 text-sm">
                        <button onclick="addTopic()" class="bg-purple-500 hover:bg-purple-600 text-white text-sm font-semibold py-1 px-3 rounded-md transition duration-150 ease-in-out">Adicionar Tema</button>
                    </div>
                     <p id="add-topic-error" class="text-red-500 text-xs mb-3"></p>
                    <div id="admin-topic-list" class="max-h-48 overflow-y-auto border rounded-md p-2 bg-gray-50 text-sm space-y-1">
                        </div>
                </div>

                <div>
                    <h3 class="font-medium mb-2 text-base">Gerenciar Questões</h3>
                     <button onclick="showAddQuestionForm()" class="bg-green-500 hover:bg-green-600 text-white text-sm font-semibold py-1 px-3 rounded-md transition duration-150 ease-in-out mb-3">Adicionar Nova Questão</button>
                     <div id="add-question-form" class="hidden border p-4 rounded-md bg-gray-50 mb-4">
                         <h4 class="font-semibold mb-3 text-md">Nova Questão</h4>
                         <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-3">
                            <div>
                                <label for="new-question-topic" class="block text-sm font-medium text-gray-700 mb-1">Tema</label>
                                <select id="new-question-topic" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                                    </select>
                            </div>
                             <div>
                                <label for="new-question-text" class="block text-sm font-medium text-gray-700 mb-1">Enunciado da Questão</label>
                                <textarea id="new-question-text" rows="2" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500"></textarea>
                             </div>
                         </div>
                         <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-3">
                            <div>
                                <label for="new-option-a" class="block text-sm font-medium text-gray-700 mb-1">Opção A</label>
                                <input type="text" id="new-option-a" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                            </div>
                             <div>
                                <label for="new-option-b" class="block text-sm font-medium text-gray-700 mb-1">Opção B</label>
                                <input type="text" id="new-option-b" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                            </div>
                             <div>
                                <label for="new-option-c" class="block text-sm font-medium text-gray-700 mb-1">Opção C</label>
                                <input type="text" id="new-option-c" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                            </div>
                             <div>
                                <label for="new-option-d" class="block text-sm font-medium text-gray-700 mb-1">Opção D</label>
                                <input type="text" id="new-option-d" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                            </div>
                         </div>
                         <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-3">
                             <div>
                                <label for="new-correct-option" class="block text-sm font-medium text-gray-700 mb-1">Alternativa Correta</label>
                                <select id="new-correct-option" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                                    <option value="A">A</option>
                                    <option value="B">B</option>
                                    <option value="C">C</option>
                                    <option value="D">D</option>
                                </select>
                             </div>
                             <div>
                                <label for="new-explanation" class="block text-sm font-medium text-gray-700 mb-1">Comentário/Explicação</label>
                                <textarea id="new-explanation" rows="2" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500"></textarea>
                             </div>
                         </div>
                         <div class="flex justify-end gap-2">
                             <button onclick="cancelAddQuestion()" class="bg-gray-300 hover:bg-gray-400 text-gray-800 text-sm font-semibold py-1 px-3 rounded-md transition duration-150 ease-in-out">Cancelar</button>
                             <button onclick="addQuestion()" class="bg-blue-500 hover:bg-blue-600 text-white text-sm font-semibold py-1 px-3 rounded-md transition duration-150 ease-in-out">Salvar Questão</button>
                         </div>
                         <p id="add-question-error" class="text-red-500 text-sm mt-2"></p>
                     </div>

                    <div id="admin-question-list" class="max-h-96 overflow-y-auto">
                        </div>
                </div>
            </div>

            <div id="student-view" class="hidden">
                <div id="topic-selection" class="bg-white p-6 rounded-lg shadow-md mb-6">
                    <h2 class="text-lg font-semibold mb-4">Escolha um Tema para Praticar</h2>
                    <div id="topic-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                        </div>
                     <div id="student-progress" class="mt-6 border-t pt-4">
                         <h3 class="font-medium mb-2 text-md">Seu Progresso Geral</h3>
                         <p id="progress-summary">Nenhuma questão respondida ainda.</p>
                         </div>
                </div>

                <div id="quiz-area" class="hidden bg-white p-6 rounded-lg shadow-md">
                    <div class="flex justify-between items-center mb-4">
                         <h2 class="text-lg font-semibold">Questão <span id="question-number"></span> de <span id="total-questions"></span> (<span id="current-topic-name"></span>)</h2>
                         <button onclick="backToTopicSelection()" class="text-sm text-blue-600 hover:underline">Voltar aos Temas</button>
                    </div>

                    <div id="question-display" class="mb-5">
                        <p id="question-text" class="mb-4 text-gray-700 text-base"></p>
                        <div id="options-container" class="space-y-3">
                            </div>
                    </div>

                    <div id="feedback-area" class="mt-4 p-4 rounded-md hidden border">
                        <p id="feedback-text" class="font-bold text-lg mb-2"></p>
                        <p class="text-sm font-semibold mt-3 mb-1">Comentário:</p>
                        <p id="explanation-text" class="text-sm text-gray-700 italic bg-gray-100 p-3 rounded"></p>
                    </div>

                    <div class="mt-6 flex justify-end">
                        <button id="confirm-button" onclick="confirmAnswer()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md transition duration-150 ease-in-out mr-2">Confirmar</button>
                        <button id="next-button" onclick="nextQuestion()" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-md transition duration-150 ease-in-out hidden">Próxima Questão</button>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- Mock Data ---
        let db = {
            // Users: Simple email/password/role mapping
            users: {
                "admin@teste.com": { password: "admin", role: "admin", name: "Admin" },
                "aluno@teste.com": { password: "aluno", role: "student", name: "Aluno Teste" }
            },
            // Topics
            topics: [
                { id: 1, name: "Vitaminas", description: "Questões sobre vitaminas lipossolúveis e hidrossolúveis." },
                { id: 2, name: "Obesidade", description: "Abordagem clínica e tratamento da obesidade." },
                { id: 3, name: "Nutrição Enteral", description: "Indicações e manejo da nutrição enteral." }
            ],
            // Questions
            questions: [
                { id: 1, topicId: 1, text: "Qual vitamina é essencial para a coagulação sanguínea?", optionA: "Vitamina A", optionB: "Vitamina D", optionC: "Vitamina K", optionD: "Vitamina E", correctOption: "C", explanation: "A vitamina K é crucial para a síntese de fatores de coagulação no fígado." , createdBy: "admin@teste.com"},
                { id: 2, topicId: 1, text: "A deficiência de qual vitamina causa Escorbuto?", optionA: "Vitamina C", optionB: "Vitamina B1", optionC: "Vitamina B12", optionD: "Vitamina A", correctOption: "A", explanation: "O escorbuto é causado pela deficiência grave de Vitamina C (ácido ascórbico)." , createdBy: "admin@teste.com"},
                { id: 3, topicId: 2, text: "Qual o IMC que classifica Obesidade Grau I?", optionA: "25-29.9 kg/m²", optionB: "30-34.9 kg/m²", optionC: "35-39.9 kg/m²", optionD: "> 40 kg/m²", correctOption: "B", explanation: "Obesidade Grau I é definida por um Índice de Massa Corporal (IMC) entre 30 e 34.9 kg/m²." , createdBy: "admin@teste.com"},
                { id: 4, topicId: 2, text: "Qual medicamento é um inibidor da lipase pancreática usado no tratamento da obesidade?", optionA: "Metformina", optionB: "Sibutramina", optionC: "Orlistat", optionD: "Liraglutida", correctOption: "C", explanation: "Orlistat age inibindo a absorção de gorduras no intestino." , createdBy: "admin@teste.com"},
                { id: 5, topicId: 3, text: "Qual a via de administração preferencial para nutrição enteral, quando possível?", optionA: "Jejunostomia", optionB: "Gastrostomia", optionC: "Sonda Nasogástrica", optionD: "Sonda Nasoenteral", correctOption: "C", explanation: "A sonda nasogástrica é geralmente a via inicial e preferencial por ser menos invasiva, se o estômago estiver funcional." , createdBy: "admin@teste.com"},
            ],
            // Student progress (simple version, stored in memory)
            studentProgress: {
                // "aluno@teste.com": { topicId: { correct: 0, total: 0 }, ... }
            }
        };

        // --- Application State ---
        let currentUser = null; // { email: '...', role: '...', name: '...' }
        let currentQuiz = {
            topicId: null,
            questions: [],
            currentQuestionIndex: 0,
            score: 0,
            selectedAnswer: null
        };

        // --- DOM Elements ---
        const loginScreen = document.getElementById('login-screen');
        const mainContent = document.getElementById('main-content');
        const adminView = document.getElementById('admin-view');
        const studentView = document.getElementById('student-view');
        const topicSelection = document.getElementById('topic-selection');
        const quizArea = document.getElementById('quiz-area');
        const userInfo = document.getElementById('user-info');
        const loginError = document.getElementById('login-error');
        const topicList = document.getElementById('topic-list');
        const questionNumber = document.getElementById('question-number');
        const totalQuestions = document.getElementById('total-questions');
        const currentTopicName = document.getElementById('current-topic-name');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const feedbackArea = document.getElementById('feedback-area');
        const feedbackText = document.getElementById('feedback-text');
        // const correctAnswerText = document.getElementById('correct-answer-text'); // Removed, feedback is visual now
        const explanationText = document.getElementById('explanation-text');
        const confirmButton = document.getElementById('confirm-button');
        const nextButton = document.getElementById('next-button');
        const adminQuestionList = document.getElementById('admin-question-list');
        const addQuestionForm = document.getElementById('add-question-form');
        const newQuestionTopicSelect = document.getElementById('new-question-topic');
        const addQuestionError = document.getElementById('add-question-error');
        const studentProgressDiv = document.getElementById('student-progress');
        const progressSummary = document.getElementById('progress-summary');
        const adminTopicList = document.getElementById('admin-topic-list'); // Added
        const addTopicError = document.getElementById('add-topic-error'); // Added


        // --- Functions ---

        // Initialization
        function init() {
            logout(); // Start logged out
            // Initial population of dynamic elements happens on login now
        }

        // Login Logic
        function login() {
            const email = document.getElementById('email').value.trim();
            const password = document.getElementById('password').value;
            loginError.textContent = '';

            const user = db.users[email];

            if (user && user.password === password) {
                currentUser = { email: email, role: user.role, name: user.name };
                showMainContent();
            } else {
                loginError.textContent = 'Email ou senha inválidos.';
            }
        }

        // Logout Logic
        function logout() {
            currentUser = null;
            loginScreen.style.display = 'block';
            mainContent.style.display = 'none';
            adminView.style.display = 'none';
            studentView.style.display = 'none';
            quizArea.style.display = 'none';
            topicSelection.style.display = 'block';
            document.getElementById('email').value = '';
            document.getElementById('password').value = '';
            loginError.textContent = '';
            cancelAddQuestion(); // Ensure forms are hidden
        }

        // Show Main Content Area after Login
        function showMainContent() {
            loginScreen.style.display = 'none';
            mainContent.style.display = 'block';
            userInfo.textContent = `Logado como: ${currentUser.name} (${currentUser.role})`;

            if (currentUser.role === 'admin') {
                adminView.style.display = 'block';
                studentView.style.display = 'none';
                // Populate admin specific elements
                populateAdminTopicSelect();
                displayAdminTopics();
                displayAdminQuestions();
            } else {
                adminView.style.display = 'none';
                studentView.style.display = 'block';
                quizArea.style.display = 'none';
                topicSelection.style.display = 'block';
                // Populate student specific elements
                displayTopics();
                updateStudentProgressDisplay();
            }
        }

        // --- Admin Functions ---

        // NEW: Display list of topics in Admin view
        function displayAdminTopics() {
            adminTopicList.innerHTML = ''; // Clear list
             if (db.topics.length === 0) {
                 adminTopicList.innerHTML = '<p class="text-gray-500 italic px-1">Nenhum tema cadastrado.</p>';
                 return;
            }
            db.topics.forEach(topic => {
                const topicElement = document.createElement('div');
                topicElement.className = 'flex justify-between items-center py-1 px-2 rounded hover:bg-gray-200';
                topicElement.innerHTML = `
                    <span>${topic.name}</span>
                    <button onclick="deleteTopic(${topic.id})" class="text-red-500 hover:text-red-700 text-xs font-semibold ml-2">Excluir</button>
                `;
                 // Add delete functionality later if needed
                adminTopicList.appendChild(topicElement);
            });
        }

        // NEW: Add a new topic
        function addTopic() {
            const topicNameInput = document.getElementById('new-topic-name');
            const topicName = topicNameInput.value.trim();
            addTopicError.textContent = ''; // Clear previous errors

            if (!topicName) {
                addTopicError.textContent = 'Por favor, insira um nome para o tema.';
                return;
            }
            // Check if topic already exists (case-insensitive)
            if (db.topics.some(topic => topic.name.toLowerCase() === topicName.toLowerCase())) {
                 addTopicError.textContent = 'Este tema já existe.';
                 return;
            }

            const newId = db.topics.length > 0 ? Math.max(...db.topics.map(t => t.id)) + 1 : 1;
            const newTopic = {
                id: newId,
                name: topicName,
                description: "" // Add description field later if needed
            };

            db.topics.push(newTopic);
            console.log("Tema adicionado (simulado):", newTopic);

            // Refresh displays
            displayAdminTopics();
            populateAdminTopicSelect(); // Update the dropdown for adding questions
            if(currentUser.role === 'student') displayTopics(); // Update student view if somehow visible

            topicNameInput.value = ''; // Clear input field
            // Note: In a real app, this would be an API call.
        }

        // NEW: Delete a topic (and its questions - requires confirmation)
        function deleteTopic(topicId) {
            const topic = db.topics.find(t => t.id === topicId);
            if (!topic) return;

            const questionsInTopic = db.questions.filter(q => q.topicId === topicId).length;
            let confirmationMessage = `Tem certeza que deseja excluir o tema "${topic.name}"?`;
            if (questionsInTopic > 0) {
                confirmationMessage += `\n\nATENÇÃO: ${questionsInTopic} questão(ões) associada(s) a este tema também serão excluídas!`;
            }
             confirmationMessage += "\n\nEsta ação é simulada e não pode ser desfeita.";


            if (confirm(confirmationMessage)) {
                // Filter out the topic
                db.topics = db.topics.filter(t => t.id !== topicId);
                // Filter out questions associated with the topic
                db.questions = db.questions.filter(q => q.topicId !== topicId);

                console.log(`Tema ${topicId} e suas questões excluídos (simulado).`);

                // Refresh displays
                displayAdminTopics();
                populateAdminTopicSelect();
                displayAdminQuestions(); // Refresh question list as well
                 if(currentUser.role === 'student') displayTopics(); // Update student view if somehow visible
                 // Note: In a real app, this would be multiple API calls or a cascading delete.
            }
        }


        // Populate the select dropdown in the "Add Question" form
        function populateAdminTopicSelect() {
            newQuestionTopicSelect.innerHTML = '<option value="">Selecione um Tema</option>'; // Clear existing
            db.topics.forEach(topic => {
                const option = document.createElement('option');
                option.value = topic.id;
                option.textContent = topic.name;
                newQuestionTopicSelect.appendChild(option);
            });
        }

        // Display list of questions in Admin view
        function displayAdminQuestions() {
            adminQuestionList.innerHTML = ''; // Clear list
            if (db.questions.length === 0) {
                 adminQuestionList.innerHTML = '<p class="text-gray-500 italic">Nenhuma questão cadastrada.</p>';
                 return;
            }

            // Sort questions maybe by topic then text? Optional.
            // db.questions.sort((a, b) => a.topicId - b.topicId || a.text.localeCompare(b.text));


            db.questions.forEach((q, index) => {
                const topic = db.topics.find(t => t.id === q.topicId);
                const questionElement = document.createElement('div');
                questionElement.className = 'border p-3 rounded-md mb-2 bg-gray-50 text-sm';
                questionElement.innerHTML = `
                    <p class="font-semibold">${index + 1}. [${topic ? topic.name : 'Sem Tema'}] ${q.text}</p>
                    <ul class="list-disc list-inside ml-4 my-1 text-xs">
                        <li>A: ${q.optionA}</li>
                        <li>B: ${q.optionB}</li>
                        <li>C: ${q.optionC}</li>
                        <li>D: ${q.optionD}</li>
                    </ul>
                    <p class="text-green-600 font-medium text-xs">Correta: ${q.correctOption}</p>
                    <p class="text-gray-600 italic mt-1 text-xs">Explicação: ${q.explanation || 'N/A'}</p>
                    <div class="text-right mt-1">
                        <button onclick="deleteQuestion(${q.id})" class="text-red-500 hover:text-red-700 text-xs font-semibold">Excluir</button>
                        </div>
                `;
                adminQuestionList.appendChild(questionElement);
            });
        }

         function showAddQuestionForm() {
            addQuestionForm.style.display = 'block';
            addQuestionError.textContent = '';
            document.getElementById('new-question-topic').value = '';
            document.getElementById('new-question-text').value = '';
            document.getElementById('new-option-a').value = '';
            document.getElementById('new-option-b').value = '';
            document.getElementById('new-option-c').value = '';
            document.getElementById('new-option-d').value = '';
            document.getElementById('new-correct-option').value = 'A';
            document.getElementById('new-explanation').value = '';
        }

        function cancelAddQuestion() {
            addQuestionForm.style.display = 'none';
            addQuestionError.textContent = '';
        }

        function addQuestion() {
            const topicId = parseInt(document.getElementById('new-question-topic').value);
            const text = document.getElementById('new-question-text').value.trim();
            const optionA = document.getElementById('new-option-a').value.trim();
            const optionB = document.getElementById('new-option-b').value.trim();
            const optionC = document.getElementById('new-option-c').value.trim();
            const optionD = document.getElementById('new-option-d').value.trim();
            const correctOption = document.getElementById('new-correct-option').value;
            const explanation = document.getElementById('new-explanation').value.trim();
            addQuestionError.textContent = '';

            if (!topicId || !text || !optionA || !optionB || !optionC || !optionD || !correctOption) {
                addQuestionError.textContent = 'Erro: Todos os campos da questão são obrigatórios (exceto explicação).';
                return;
            }

            const newId = db.questions.length > 0 ? Math.max(...db.questions.map(q => q.id)) + 1 : 1;
            const newQuestion = {
                id: newId, topicId, text, optionA, optionB, optionC, optionD, correctOption, explanation, createdBy: currentUser.email
            };

            db.questions.push(newQuestion);
            displayAdminQuestions();
            cancelAddQuestion();
            console.log("Questão adicionada (simulado):", newQuestion);
        }

        function deleteQuestion(questionId) {
            if (confirm('Tem certeza que deseja excluir esta questão? Esta ação é simulada.')) {
                const indexToDelete = db.questions.findIndex(q => q.id === questionId);
                if (indexToDelete > -1) {
                    db.questions.splice(indexToDelete, 1);
                    displayAdminQuestions();
                    console.log(`Questão ${questionId} excluída (simulado).`);
                } else {
                    console.error(`Questão com ID ${questionId} não encontrada.`);
                }
            }
        }


        // --- Student Functions ---

        function displayTopics() {
            topicList.innerHTML = '';
            db.topics.forEach(topic => {
                const button = document.createElement('button');
                button.textContent = topic.name;
                button.className = 'bg-white hover:bg-blue-50 border border-gray-300 text-blue-700 font-semibold py-4 px-4 rounded-lg shadow transition duration-150 ease-in-out text-center';
                button.onclick = () => startQuiz(topic.id);
                topicList.appendChild(button);
            });
             if (db.topics.length === 0) {
                 topicList.innerHTML = '<p class="text-gray-500 italic col-span-full text-center">Nenhum tema disponível no momento.</p>';
            }
        }

        function updateStudentProgressDisplay() {
            const userProgress = db.studentProgress[currentUser.email];
            let progressContainerHtml = '<h3 class="font-medium mb-2 text-md">Seu Progresso por Tema</h3>';

            if (!userProgress || Object.keys(userProgress).length === 0) {
                progressSummary.textContent = 'Nenhuma questão respondida ainda.';
                 studentProgressDiv.innerHTML = progressContainerHtml + '<p class="text-sm text-gray-500">Responda algumas questões para ver seu progresso aqui.</p>'; // Clear detailed list
                return;
            }

            let totalCorrect = 0;
            let totalAnswered = 0;
            let progressListHtml = '<ul class="list-disc list-inside space-y-1 text-sm">';

            for (const topicId in userProgress) {
                const progress = userProgress[topicId];
                const topic = db.topics.find(t => t.id == topicId); // Use == for type coercion just in case
                if (topic) { // Only display progress for topics that still exist
                    const percentage = progress.total > 0 ? ((progress.correct / progress.total) * 100).toFixed(0) : 0;
                    totalCorrect += progress.correct;
                    totalAnswered += progress.total;
                    progressListHtml += `<li>${topic.name}: ${progress.correct} de ${progress.total} (${percentage}%)</li>`;
                }
            }
             progressListHtml += '</ul>';

             const overallPercentage = totalAnswered > 0 ? ((totalCorrect / totalAnswered) * 100).toFixed(0) : 0;
             progressSummary.innerHTML = `Progresso Geral: ${totalCorrect} de ${totalAnswered} questões corretas (${overallPercentage}%).`;
             studentProgressDiv.innerHTML = progressContainerHtml + progressListHtml; // Display detailed progress
        }


        function startQuiz(topicId) {
            const questionsForTopic = db.questions.filter(q => q.topicId === topicId);
            const topic = db.topics.find(t => t.id === topicId);

            if (!topic) {
                 alert("Erro: Tema não encontrado."); // Should not happen if UI is correct
                 return;
            }
            if (questionsForTopic.length === 0) {
                alert(`Não há questões disponíveis para o tema "${topic.name}".`);
                return;
            }

            currentQuiz = {
                topicId: topicId,
                questions: shuffleArray([...questionsForTopic]),
                currentQuestionIndex: 0,
                score: 0,
                selectedAnswer: null
            };

            topicSelection.style.display = 'none';
            quizArea.style.display = 'block';
            currentTopicName.textContent = topic.name;
            loadQuestion();
        }

        function loadQuestion() {
            const question = currentQuiz.questions[currentQuiz.currentQuestionIndex];
            currentQuiz.selectedAnswer = null;

            questionNumber.textContent = currentQuiz.currentQuestionIndex + 1;
            totalQuestions.textContent = currentQuiz.questions.length;
            questionText.textContent = question.text;

            optionsContainer.innerHTML = '';
            const options = [
                { key: 'A', text: question.optionA }, { key: 'B', text: question.optionB },
                { key: 'C', text: question.optionC }, { key: 'D', text: question.optionD }
            ];

            options.forEach(option => {
                const label = document.createElement('label');
                // Store the option key in a data attribute for easier selection later
                label.dataset.optionKey = option.key;
                label.className = 'flex items-center p-3 border rounded-md hover:bg-gray-100 cursor-pointer transition duration-150 ease-in-out';
                label.innerHTML = `
                    <input type="radio" name="option" value="${option.key}" class="mr-3 h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500">
                    <span class="font-medium">${option.key})</span> <span class="ml-2 text-sm">${option.text}</span>
                `;
                // Add event listener to update selectedAnswer and provide visual feedback on click
                label.addEventListener('click', () => {
                    // Remove highlight from previously selected option if any
                     const previouslySelected = optionsContainer.querySelector('.ring-2');
                     if (previouslySelected) {
                         previouslySelected.classList.remove('ring-2', 'ring-blue-400');
                     }
                     // Add highlight to current selection
                     label.classList.add('ring-2', 'ring-blue-400');
                     currentQuiz.selectedAnswer = option.key;
                });
                optionsContainer.appendChild(label);
            });

            feedbackArea.style.display = 'none';
            confirmButton.style.display = 'inline-block';
            nextButton.style.display = 'none';
            disableOptions(false);
        }

        function confirmAnswer() {
            if (currentQuiz.selectedAnswer === null) {
                alert('Por favor, selecione uma alternativa.');
                return;
            }

            const question = currentQuiz.questions[currentQuiz.currentQuestionIndex];
            const isCorrect = currentQuiz.selectedAnswer === question.correctOption;

            feedbackArea.style.display = 'block';
            feedbackArea.classList.remove('border-green-500', 'border-red-500'); // Clear previous border colors

            if (isCorrect) {
                feedbackText.textContent = 'Resposta Correta!';
                feedbackText.className = 'font-bold text-lg mb-2 text-green-600'; // Style text color
                feedbackArea.classList.add('border-green-500'); // Style border color
                currentQuiz.score++;
            } else {
                feedbackText.textContent = 'Resposta Incorreta!';
                feedbackText.className = 'font-bold text-lg mb-2 text-red-600'; // Style text color
                 feedbackArea.classList.add('border-red-500'); // Style border color
            }

            // Highlight correct and incorrect options visually
            optionsContainer.querySelectorAll('label').forEach(label => {
                const optionKey = label.dataset.optionKey;
                 // Remove selection ring if present
                 label.classList.remove('ring-2', 'ring-blue-400');

                if (optionKey === question.correctOption) {
                    label.classList.add('correct-answer-highlight', 'selected-answer-border'); // Green background and border
                } else if (optionKey === currentQuiz.selectedAnswer && !isCorrect) {
                    label.classList.add('incorrect-answer-highlight', 'selected-answer-border'); // Red background and border for wrong selection
                }
            });


            // Display explanation
            explanationText.textContent = `${question.explanation || 'Nenhuma explicação disponível.'}`;

            updateProgress(question.topicId, isCorrect);

            confirmButton.style.display = 'none';
            if (currentQuiz.currentQuestionIndex < currentQuiz.questions.length - 1) {
                nextButton.style.display = 'inline-block';
                 nextButton.textContent = 'Próxima Questão';
            } else {
                nextButton.textContent = 'Ver Resultados';
                nextButton.style.display = 'inline-block';
            }
            disableOptions(true);
        }

        function updateProgress(topicId, isCorrect) {
            if (!currentUser || currentUser.role !== 'student') return; // Only track for logged-in students

            if (!db.studentProgress[currentUser.email]) {
                db.studentProgress[currentUser.email] = {};
            }
            if (!db.studentProgress[currentUser.email][topicId]) {
                db.studentProgress[currentUser.email][topicId] = { correct: 0, total: 0 };
            }

            const progress = db.studentProgress[currentUser.email][topicId];
            progress.total++;
            if (isCorrect) {
                progress.correct++;
            }
            console.log("Progresso atualizado (simulado):", db.studentProgress[currentUser.email]);
            // updateStudentProgressDisplay(); // Update happens when going back to topic list or finishing quiz
        }


        function nextQuestion() {
            currentQuiz.currentQuestionIndex++;
            if (currentQuiz.currentQuestionIndex < currentQuiz.questions.length) {
                loadQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            quizArea.style.display = 'none';
            topicSelection.style.display = 'block';

            const percentage = currentQuiz.questions.length > 0 ? ((currentQuiz.score / currentQuiz.questions.length) * 100).toFixed(0) : 0;
            updateStudentProgressDisplay(); // Update display now

            // Optional: Show a summary alert/modal
            alert(`Quiz Concluído!\nTema: ${currentTopicName.textContent}\nPontuação: ${currentQuiz.score} de ${currentQuiz.questions.length} (${percentage}%)\n\nSeu progresso foi atualizado. Escolha outro tema ou revise seu progresso.`);
        }

        function backToTopicSelection() {
             if (currentQuiz.topicId !== null && currentQuiz.currentQuestionIndex < currentQuiz.questions.length) { // Only ask if quiz is in progress
                 if (!confirm('Tem certeza que deseja sair deste quiz? Seu progresso neste tema será salvo, mas o quiz atual será interrompido.')) {
                     return;
                 }
             }
            quizArea.style.display = 'none';
            topicSelection.style.display = 'block';
             updateStudentProgressDisplay(); // Ensure progress is up-to-date
             currentQuiz.topicId = null; // Reset quiz state
        }

        function disableOptions(disabled) {
            optionsContainer.querySelectorAll('input[type="radio"]').forEach(input => {
                input.disabled = disabled;
                 const label = input.closest('label');
                 if(label) {
                     label.classList.toggle('opacity-60', disabled);
                     label.classList.toggle('cursor-not-allowed', disabled);
                     label.classList.toggle('hover:bg-gray-100', !disabled);
                     // Prevent clicking the label itself when disabled
                     if(disabled) {
                         label.style.pointerEvents = 'none';
                     } else {
                         label.style.pointerEvents = 'auto';
                     }
                 }
            });
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }


        // --- Run Initialization ---
        init();

    </script>

</body>
</html>
