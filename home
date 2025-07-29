<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Claude - Asistente de Voz Automático</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            padding: 20px;
        }

        .container {
            text-align: center;
            max-width: 800px;
            width: 100%;
        }

        h1 {
            font-size: 3em;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .status {
            font-size: 1.5em;
            margin-bottom: 30px;
            padding: 20px;
            border-radius: 15px;
            background: rgba(255,255,255,0.2);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.3);
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .claude-indicator {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            margin: 0 auto 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4em;
            background: rgba(255,255,255,0.1);
            border: 3px solid rgba(255,255,255,0.3);
            transition: all 0.3s ease;
        }

        .claude-indicator.listening {
            animation: pulseListening 2s infinite;
            background: rgba(255,107,107,0.3);
            border-color: #ff6b6b;
        }

        .claude-indicator.responding {
            animation: pulseResponding 1s infinite;
            background: rgba(76,175,80,0.3);
            border-color: #4caf50;
        }

        @keyframes pulseListening {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0.7); }
            70% { transform: scale(1.05); box-shadow: 0 0 0 10px rgba(255,107,107,0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0); }
        }

        @keyframes pulseResponding {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        button {
            padding: 15px 30px;
            font-size: 1.1em;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            font-weight: bold;
        }

        .start-btn {
            background: linear-gradient(45deg, #4caf50, #45a049);
            color: white;
        }

        .start-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(76,175,80,0.4);
        }

        .stop-btn {
            background: linear-gradient(45deg, #f44336, #d32f2f);
            color: white;
        }

        .stop-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(244,67,54,0.4);
        }

        .response-area {
            background: rgba(255,255,255,0.1);
            border-radius: 20px;
            padding: 30px;
            margin-top: 20px;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255,255,255,0.2);
            min-height: 200px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .response-text {
            font-size: 1.4em;
            line-height: 1.6;
            text-align: left;
            max-height: 300px;
            overflow-y: auto;
        }

        .wake-word-display {
            font-size: 1.8em;
            color: #ff6b6b;
            font-weight: bold;
            margin: 20px 0;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .volume-controls {
            margin-top: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        .volume-slider {
            width: 200px;
            height: 8px;
            border-radius: 5px;
            background: rgba(255,255,255,0.3);
            outline: none;
            -webkit-appearance: none;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: #ff6b6b;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }

        .error {
            color: #ff6b6b;
            background: rgba(255,107,107,0.1);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            border: 1px solid rgba(255,107,107,0.3);
        }

        .conversation-log {
            background: rgba(0,0,0,0.2);
            border-radius: 15px;
            padding: 20px;
            margin-top: 20px;
            max-height: 300px;
            overflow-y: auto;
            text-align: left;
        }

        .conversation-entry {
            margin-bottom: 15px;
            padding: 10px;
            border-radius: 10px;
        }

        .user-entry {
            background: rgba(255,255,255,0.1);
            border-left: 4px solid #2196f3;
        }

        .claude-entry {
            background: rgba(255,255,255,0.05);
            border-left: 4px solid #4caf50;
        }

        @media (max-width: 600px) {
            h1 { font-size: 2em; }
            .claude-indicator { width: 120px; height: 120px; font-size: 3em; }
            button { padding: 12px 25px; font-size: 1em; }
            .response-text { font-size: 1.2em; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🤖 Claude - Asistente Inteligente</h1>
        
        <div class="claude-indicator" id="claudeIndicator">
            🎤
        </div>

        <div class="wake-word-display">
            Di: <strong>"Hola Claude"</strong> para comenzar
        </div>
        
        <div class="status" id="status">
            Presiona "Iniciar" para que Claude comience a escuchar siempre
        </div>

        <div class="controls">
            <button class="start-btn" id="startBtn">🚀 Iniciar Claude</button>
            <button class="stop-btn" id="stopBtn" style="display: none;">⏹️ Detener</button>
        </div>

        <div class="response-area">
            <div class="response-text" id="responseText">
                Una vez iniciado, Claude estará siempre escuchando. Solo di "Hola Claude" seguido de tu pregunta, como: "Hola Claude, ¿qué hora es?" o "Hola Claude, cuéntame un chiste".
            </div>
        </div>

        <div class="volume-controls">
            <span>🔊 Volumen:</span>
            <input type="range" class="volume-slider" id="volumeSlider" min="0" max="1" step="0.1" value="0.8">
            <span id="volumeDisplay">80%</span>
        </div>

        <div class="conversation-log" id="conversationLog" style="display: none;">
            <h3>📝 Conversación:</h3>
            <div id="logEntries"></div>
        </div>
    </div>

    <script>
        class AutoClaudeVoiceAssistant {
            constructor() {
                this.isActive = false;
                this.isListening = false;
                this.isSpeaking = false;
                this.isProcessing = false;
                this.recognition = null;
                this.synthesis = window.speechSynthesis;
                this.volume = 0.8;
                this.wakeWords = ['hola claude', 'claude', 'oye claude'];
                this.conversationHistory = [];
                
                this.initElements();
                this.initSpeechRecognition();
                this.initEventListeners();
                this.setupVoiceSettings();
            }

            initElements() {
                this.startBtn = document.getElementById('startBtn');
                this.stopBtn = document.getElementById('stopBtn');
                this.status = document.getElementById('status');
                this.responseText = document.getElementById('responseText');
                this.claudeIndicator = document.getElementById('claudeIndicator');
                this.volumeSlider = document.getElementById('volumeSlider');
                this.volumeDisplay = document.getElementById('volumeDisplay');
                this.conversationLog = document.getElementById('conversationLog');
                this.logEntries = document.getElementById('logEntries');
            }

            initSpeechRecognition() {
                if ('webkitSpeechRecognition' in window) {
                    this.recognition = new webkitSpeechRecognition();
                } else if ('SpeechRecognition' in window) {
                    this.recognition = new SpeechRecognition();
                } else {
                    this.showError('Tu navegador no soporta reconocimiento de voz. Intenta con Chrome o Edge.');
                    return;
                }

                // Configuración para escucha continua
                this.recognition.continuous = true;
                this.recognition.interimResults = true;
                this.recognition.lang = 'es-ES';
                this.recognition.maxAlternatives = 1;

                this.recognition.onstart = () => {
                    this.isListening = true;
                    this.updateIndicator();
                };

                this.recognition.onresult = (event) => {
                    let finalTranscript = '';
                    let interimTranscript = '';

                    for (let i = event.resultIndex; i < event.results.length; i++) {
                        const transcript = event.results[i][0].transcript;
                        if (event.results[i].isFinal) {
                            finalTranscript += transcript;
                        } else {
                            interimTranscript += transcript;
                        }
                    }

                    if (finalTranscript) {
                        this.processTranscript(finalTranscript.toLowerCase().trim());
                    }
                };

                this.recognition.onerror = (event) => {
                    console.log('Error en reconocimiento:', event.error);
                    if (event.error === 'network') {
                        // Reintentar automáticamente
                        setTimeout(() => this.restartListening(), 1000);
                    }
                };

                this.recognition.onend = () => {
                    if (this.isActive && !this.isProcessing) {
                        // Reiniciar automáticamente si está activo
                        setTimeout(() => this.restartListening(), 500);
                    }
                };
            }

            initEventListeners() {
                this.startBtn.addEventListener('click', () => this.startClaude());
                this.stopBtn.addEventListener('click', () => this.stopClaude());
                
                this.volumeSlider.addEventListener('input', (e) => {
                    this.volume = parseFloat(e.target.value);
                    this.volumeDisplay.textContent = Math.round(this.volume * 100) + '%';
                });

                // Prevenir que la página se cierre accidentalmente
                window.addEventListener('beforeunload', (e) => {
                    if (this.isActive) {
                        e.preventDefault();
                        e.returnValue = '';
                    }
                });
            }

            setupVoiceSettings() {
                this.synthesis.onvoiceschanged = () => {
                    const voices = this.synthesis.getVoices();
                    this.spanishVoice = voices.find(voice => 
                        voice.lang.includes('es') && voice.name.includes('Google')
                    ) || voices.find(voice => 
                        voice.lang.includes('es')
                    ) || voices[0];
                };
            }

            startClaude() {
                this.isActive = true;
                this.startBtn.style.display = 'none';
                this.stopBtn.style.display = 'inline-block';
                this.conversationLog.style.display = 'block';
                
                this.updateStatus('🎤 Claude está activo y escuchando. Di "Hola Claude" para hablar.');
                this.startListening();
                
                // Saludo inicial
                setTimeout(() => {
                    this.speakResponse('Hola, soy Claude. Estoy listo para ayudarte. Solo di "Hola Claude" seguido de tu pregunta y te responderé.');
                }, 1000);
            }

            stopClaude() {
                this.isActive = false;
                this.isProcessing = false;
                this.startBtn.style.display = 'inline-block';
                this.stopBtn.style.display = 'none';
                
                if (this.recognition) {
                    this.recognition.stop();
                }
                
                if (this.synthesis.speaking) {
                    this.synthesis.cancel();
                }
                
                this.updateStatus('Claude detenido. Presiona "Iniciar" para reactivar.');
                this.updateIndicator();
            }

            startListening() {
                if (!this.recognition || !this.isActive) return;
                
                try {
                    this.recognition.start();
                } catch (error) {
                    console.log('Error al iniciar reconocimiento:', error);
                    setTimeout(() => this.restartListening(), 1000);
                }
            }

            restartListening() {
                if (!this.isActive) return;
                
                try {
                    this.recognition.start();
                } catch (error) {
                    // Si ya está corriendo, no hacer nada
                    if (error.name !== 'InvalidStateError') {
                        setTimeout(() => this.restartListening(), 2000);
                    }
                }
            }

            processTranscript(transcript) {
                // Buscar palabra de activación
                const hasWakeWord = this.wakeWords.some(wake => transcript.includes(wake));
                
                if (hasWakeWord && !this.isProcessing) {
                    this.isProcessing = true;
                    this.claudeIndicator.classList.add('responding');
                    
                    // Extraer el comando después de la palabra de activación
                    let command = transcript;
                    this.wakeWords.forEach(wake => {
                        if (transcript.includes(wake)) {
                            command = transcript.replace(wake, '').trim();
                        }
                    });

                    if (command.length > 0) {
                        this.updateStatus(`🗣️ Escuché: "${command}"`);
                        this.addToConversation('user', command);
                        this.processUserCommand(command);
                    } else {
                        this.updateStatus('👋 Hola, ¿en qué puedo ayudarte?');
                        this.speakResponse('Hola, ¿en qué puedo ayudarte?');
                        this.isProcessing = false;
                        this.updateIndicator();
                    }
                }
            }

            async processUserCommand(command) {
                this.updateStatus('🤔 Procesando tu solicitud...');
                
                try {
                    const response = await this.generateClaudeResponse(command);
                    this.displayResponse(response);
                    this.addToConversation('claude', response);
                    this.speakResponse(response);
                } catch (error) {
                    this.showError('Error al procesar tu solicitud');
                    this.isProcessing = false;
                    this.updateIndicator();
                }
            }

            async generateClaudeResponse(input) {
                // Simulación de respuestas inteligentes más naturales
                const lowerInput = input.toLowerCase();
                
                const responses = {
                    // Saludos y conversación
                    saludo: [
                        "¡Hola! Me alegra poder conversar contigo. ¿Cómo estás hoy?",
                        "¡Qué gusto saludarte! ¿En qué puedo ayudarte?",
                        "¡Hola! Estoy aquí para lo que necesites."
                    ],
                    
                    // Información básica
                    hora: `Son las ${new Date().toLocaleTimeString('es-ES', {hour: '2-digit', minute: '2-digit'})}`,
                    fecha: `Hoy es ${new Date().toLocaleDateString('es-ES', { 
                        weekday: 'long', 
                        day: 'numeric',
                        month: 'long'
                    })}`,
                    
                    // Clima (simulado)
                    clima: [
                        "Según mis datos, hoy está soleado con 22 grados. Perfecto para salir.",
                        "El clima está agradable hoy, cielo despejado y temperatura templada.",
                        "Hoy hace buen tiempo, ideal para dar un paseo."
                    ],
                    
                    // Entretenimiento
                    chistes: [
                        "¿Por qué los pájaros vuelan hacia el sur en invierno? Porque caminando tardarían mucho más.",
                        "¿Qué le dice un semáforo a otro? No me mires que me estoy cambiando.",
                        "¿Cómo se llama el campeón de buceo japonés? Tokofondo.",
                        "¿Por qué los peces no pagan deudas? Porque siempre están sin un peso."
                    ],
                    
                    // Estados de ánimo
                    animo: [
                        "¡Estoy muy bien, gracias por preguntar! Listo para ayudarte en lo que necesites.",
                        "Me siento genial hoy, con muchas ganas de conversar contigo.",
                        "Estoy perfecto, siempre es un placer poder hablar contigo."
                    ],
                    
                    // Despedidas
                    despedida: [
                        "¡Hasta luego! Ha sido un gusto conversar contigo.",
                        "¡Que tengas un día maravilloso! Aquí estaré cuando me necesites.",
                        "¡Adiós! Fue un placer ayudarte hoy."
                    ]
                };

                // Análisis más sofisticado del input
                if (lowerInput.includes('hola') || lowerInput.includes('buenos días') || lowerInput.includes('buenas tardes')) {
                    return this.getRandomResponse(responses.saludo);
                }
                
                if (lowerInput.includes('hora') || lowerInput.includes('qué hora')) {
                    return responses.hora;
                }
                
                if (lowerInput.includes('fecha') || lowerInput.includes('día es') || lowerInput.includes('qué día')) {
                    return responses.fecha;
                }
                
                if (lowerInput.includes('clima') || lowerInput.includes('tiempo hace') || lowerInput.includes('temperatura')) {
                    return this.getRandomResponse(responses.clima);
                }
                
                if (lowerInput.includes('chiste') || lowerInput.includes('gracioso') || lowerInput.includes('algo divertido')) {
                    return this.getRandomResponse(responses.chistes);
                }
                
                if (lowerInput.includes('cómo estás') || lowerInput.includes('cómo te sientes') || lowerInput.includes('qué tal')) {
                    return this.getRandomResponse(responses.animo);
                }
                
                if (lowerInput.includes('adiós') || lowerInput.includes('hasta luego') || lowerInput.includes('chau') || lowerInput.includes('nos vemos')) {
                    return this.getRandomResponse(responses.despedida);
                }
                
                if (lowerInput.includes('gracias')) {
                    return "¡De nada! Es un placer poder ayudarte. ¿Hay algo más en lo que pueda asistirte?";
                }
                
                if (lowerInput.includes('ayuda') || lowerInput.includes('qué puedes hacer')) {
                    return "Puedo ayudarte con muchas cosas: decirte la hora, contarte chistes, conversar contigo, darte información básica. Solo di 'Hola Claude' seguido de lo que necesites.";
                }
                
                // Respuesta por defecto más conversacional
                return `Entiendo que me dices "${input}". Aunque soy una versión simulada, estoy aquí para conversar contigo. ¿Podrías ser más específico sobre lo que necesitas?`;
            }

            getRandomResponse(responses) {
                return responses[Math.floor(Math.random() * responses.length)];
            }

            displayResponse(response) {
                this.responseText.textContent = response;
                this.updateStatus('💬 Claude respondió:');
            }

            speakResponse(text) {
                if (this.synthesis.speaking) {
                    this.synthesis.cancel();
                }

                const utterance = new SpeechSynthesisUtterance(text);
                utterance.voice = this.spanishVoice;
                utterance.volume = this.volume;
                utterance.rate = 0.9;
                utterance.pitch = 1;

                utterance.onstart = () => {
                    this.isSpeaking = true;
                    this.updateStatus('🗣️ Claude está hablando...');
                };

                utterance.onend = () => {
                    this.isSpeaking = false;
                    this.isProcessing = false;
                    this.updateStatus('👂 Escuchando... Di "Hola Claude" para hablar.');
                    this.updateIndicator();
                };

                this.synthesis.speak(utterance);
            }

            addToConversation(type, message) {
                const entry = document.createElement('div');
                entry.className = `conversation-entry ${type}-entry`;
                entry.innerHTML = `<strong>${type === 'user' ? '👤 Tú' : '🤖 Claude'}:</strong> ${message}`;
                this.logEntries.appendChild(entry);
                this.logEntries.scrollTop = this.logEntries.scrollHeight;
                
                // Mantener solo las últimas 10 entradas
                if (this.logEntries.children.length > 10) {
                    this.logEntries.removeChild(this.logEntries.firstChild);
                }
            }

            updateIndicator() {
                this.claudeIndicator.classList.remove('listening', 'responding');
                
                if (this.isProcessing || this.isSpeaking) {
                    this.claudeIndicator.classList.add('responding');
                    this.claudeIndicator.textContent = '🗣️';
                } else if (this.isActive && this.isListening) {
                    this.claudeIndicator.classList.add('listening');
                    this.claudeIndicator.textContent = '👂';
                } else {
                    this.claudeIndicator.textContent = '😴';
                }
            }

            updateStatus(message) {
                this.status.textContent = message;
            }

            showError(message) {
                this.status.innerHTML = `<div class="error">❌ ${message}</div>`;
            }
        }

        // Inicializar cuando la página esté lista
        document.addEventListener('DOMContentLoaded', () => {
            new AutoClaudeVoiceAssistant();
        });
    </script>
</body>
</html>
