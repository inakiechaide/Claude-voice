<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Claude - Asistente de Voz Simple</title>
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
            font-size: 1.3em;
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
            animation: pulseRed 2s infinite;
            background: rgba(255,107,107,0.3);
            border-color: #ff6b6b;
        }

        .claude-indicator.speaking {
            animation: pulse 1s infinite;
            background: rgba(76,175,80,0.3);
            border-color: #4caf50;
        }

        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(76,175,80,0.7); }
            70% { transform: scale(1.05); box-shadow: 0 0 0 10px rgba(76,175,80,0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(76,175,80,0); }
        }

        @keyframes pulseRed {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0.7); }
            70% { transform: scale(1.05); box-shadow: 0 0 0 10px rgba(255,107,107,0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0); }
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
            font-size: 1.2em;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            font-weight: bold;
            min-width: 150px;
        }

        .listen-btn {
            background: linear-gradient(45deg, #4caf50, #45a049);
            color: white;
        }

        .listen-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(76,175,80,0.4);
        }

        .listen-btn:active {
            transform: scale(0.95);
            background: linear-gradient(45deg, #45a049, #388e3c);
        }

        .test-btn {
            background: linear-gradient(45deg, #2196f3, #1976d2);
            color: white;
        }

        .test-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(33,150,243,0.4);
        }

        .response-area {
            background: rgba(255,255,255,0.1);
            border-radius: 20px;
            padding: 30px;
            margin-top: 20px;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255,255,255,0.2);
            min-height: 150px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .response-text {
            font-size: 1.3em;
            line-height: 1.6;
            text-align: center;
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

        .debug-info {
            background: rgba(0,0,0,0.3);
            border-radius: 10px;
            padding: 15px;
            margin-top: 20px;
            font-size: 0.9em;
            text-align: left;
            font-family: monospace;
            max-height: 200px;
            overflow-y: auto;
        }

        .error {
            color: #ff6b6b;
            background: rgba(255,107,107,0.1);
            padding: 10px;
            border-radius: 8px;
            margin: 5px 0;
            border: 1px solid rgba(255,107,107,0.3);
        }

        .success {
            color: #4caf50;
            background: rgba(76,175,80,0.1);
            padding: 10px;
            border-radius: 8px;
            margin: 5px 0;
            border: 1px solid rgba(76,175,80,0.3);
        }

        .instructions {
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            padding: 20px;
            margin-top: 20px;
            font-size: 1.1em;
            line-height: 1.5;
        }

        @media (max-width: 600px) {
            h1 { font-size: 2em; }
            .claude-indicator { width: 120px; height: 120px; font-size: 3em; }
            button { padding: 12px 20px; font-size: 1em; min-width: 120px; }
            .response-text { font-size: 1.1em; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🤖 Claude - Asistente Simple</h1>
        
        <div class="claude-indicator" id="claudeIndicator">🎤</div>
        
        <div class="status" id="status">
            Presiona "Test Micrófono" primero, luego mantén presionado "Hablar" mientras hablas
        </div>

        <div class="controls">
            <button class="test-btn" id="testBtn">🔧 Test Micrófono</button>
            <button class="listen-btn" id="listenBtn">🎤 Mantén Para Hablar</button>
        </div>

        <div class="response-area">
            <div class="response-text" id="responseText">
                <strong>INSTRUCCIONES SIMPLES:</strong><br><br>
                1️⃣ Presiona "Test Micrófono" y permite el acceso<br>
                2️⃣ Mantén presionado "Mantén Para Hablar"<br>
                3️⃣ Habla tu pregunta mientras mantienes presionado<br>
                4️⃣ Suelta el botón cuando termines<br><br>
                ¡Es como un walkie-talkie!
            </div>
        </div>

        <div class="volume-controls">
            <span>🔊 Volumen:</span>
            <input type="range" class="volume-slider" id="volumeSlider" min="0" max="1" step="0.1" value="0.8">
            <span id="volumeDisplay">80%</span>
        </div>

        <div class="instructions">
            <strong>🗣️ Ejemplos de preguntas:</strong><br>
            • "¿Qué hora es?"<br>
            • "Cuéntame un chiste"<br>
            • "¿Cómo estás?"<br>
            • "¿Qué día es hoy?"<br>
            • "Háblame del clima"
        </div>

        <div class="debug-info" id="debugInfo">
            <strong>📋 Log del sistema:</strong><br>
            <div id="debugContent">Iniciando sistema...</div>
        </div>
    </div>

    <script>
        class SimpleClaudeAssistant {
            constructor() {
                this.recognition = null;
                this.synthesis = window.speechSynthesis;
                this.volume = 0.8;
                this.isListening = false;
                this.isSpeaking = false;
                this.spanishVoice = null;
                
                this.initElements();
                this.setupSpeechRecognition();
                this.setupVoiceSettings();
                this.initEventListeners();
                this.logDebug('✅ Sistema iniciado correctamente');
                this.checkCapabilities();
            }

            initElements() {
                this.listenBtn = document.getElementById('listenBtn');
                this.testBtn = document.getElementById('testBtn');
                this.status = document.getElementById('status');
                this.responseText = document.getElementById('responseText');
                this.claudeIndicator = document.getElementById('claudeIndicator');
                this.volumeSlider = document.getElementById('volumeSlider');
                this.volumeDisplay = document.getElementById('volumeDisplay');
                this.debugContent = document.getElementById('debugContent');
                
                this.logDebug('✅ Elementos DOM cargados');
            }

            checkCapabilities() {
                const userAgent = navigator.userAgent;
                const isSafari = /Safari/.test(userAgent) && !/Chrome/.test(userAgent);
                const isIOS = /iPad|iPhone|iPod/.test(userAgent);
                
                this.logDebug(`🌐 Navegador: ${isSafari ? 'Safari' : 'Otro'}`);
                this.logDebug(`📱 iOS: ${isIOS ? 'Sí' : 'No'}`);
                this.logDebug(`🎤 SpeechRecognition: ${'webkitSpeechRecognition' in window || 'SpeechRecognition' in window ? 'Disponible' : 'No disponible'}`);
                this.logDebug(`🔊 SpeechSynthesis: ${'speechSynthesis' in window ? 'Disponible' : 'No disponible'}`);
                
                if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
                    this.showError('❌ Tu navegador no soporta reconocimiento de voz');
                    this.listenBtn.disabled = true;
                }
            }

            setupSpeechRecognition() {
                try {
                    if ('webkitSpeechRecognition' in window) {
                        this.recognition = new webkitSpeechRecognition();
                        this.logDebug('🎤 Usando webkitSpeechRecognition');
                    } else if ('SpeechRecognition' in window) {
                        this.recognition = new SpeechRecognition();
                        this.logDebug('🎤 Usando SpeechRecognition');
                    } else {
                        throw new Error('SpeechRecognition no disponible');
                    }

                    // Configuración simple para Safari
                    this.recognition.continuous = false;
                    this.recognition.interimResults = true;
                    this.recognition.lang = 'es-ES';
                    this.recognition.maxAlternatives = 1;

                    this.recognition.onstart = () => {
                        this.isListening = true;
                        this.claudeIndicator.classList.add('listening');
                        this.claudeIndicator.textContent = '👂';
                        this.updateStatus('🎤 Escuchando... habla ahora');
                        this.logDebug('🎤 Reconocimiento iniciado');
                    };

                    this.recognition.onresult = (event) => {
                        let transcript = '';
                        let isFinal = false;

                        for (let i = event.resultIndex; i < event.results.length; i++) {
                            transcript += event.results[i][0].transcript;
                            if (event.results[i].isFinal) {
                                isFinal = true;
                            }
                        }

                        this.logDebug(`📝 Transcripción: "${transcript}" (final: ${isFinal})`);
                        
                        if (transcript.trim()) {
                            this.updateStatus(`Escuché: "${transcript}"`);
                            
                            if (isFinal) {
                                this.processUserInput(transcript.trim());
                            }
                        }
                    };

                    this.recognition.onerror = (event) => {
                        this.logDebug(`❌ Error: ${event.error}`);
                        this.isListening = false;
                        this.claudeIndicator.classList.remove('listening');
                        this.claudeIndicator.textContent = '🎤';
                        
                        let errorMsg = '';
                        switch(event.error) {
                            case 'no-speech':
                                errorMsg = '❌ No se detectó voz. Intenta hablar más fuerte.';
                                break;
                            case 'audio-capture':
                                errorMsg = '❌ No se puede acceder al micrófono. Verifica los permisos.';
                                break;
                            case 'not-allowed':
                                errorMsg = '❌ Permisos de micrófono denegados. Permite el acceso.';
                                break;
                            case 'network':
                                errorMsg = '❌ Error de conexión. Verifica tu internet.';
                                break;
                            default:
                                errorMsg = `❌ Error: ${event.error}`;
                        }
                        
                        this.updateStatus(errorMsg);
                    };

                    this.recognition.onend = () => {
                        this.isListening = false;
                        this.claudeIndicator.classList.remove('listening');
                        this.claudeIndicator.textContent = '🎤';
                        this.logDebug('🎤 Reconocimiento terminado');
                        
                        if (!this.isSpeaking) {
                            this.updateStatus('✅ Listo para escuchar de nuevo');
                        }
                    };

                    this.logDebug('✅ Reconocimiento de voz configurado');

                } catch (error) {
                    this.logDebug(`❌ Error al configurar reconocimiento: ${error.message}`);
                    this.showError('❌ Error al configurar el reconocimiento de voz');
                }
            }

            setupVoiceSettings() {
                const loadVoices = () => {
                    const voices = this.synthesis.getVoices();
                    this.logDebug(`🔊 Voces disponibles: ${voices.length}`);
                    
                    // Buscar voz en español
                    this.spanishVoice = voices.find(voice => 
                        voice.lang.startsWith('es') && voice.name.includes('Google')
                    ) || voices.find(voice => 
                        voice.lang.startsWith('es')
                    ) || voices[0];
                    
                    if (this.spanishVoice) {
                        this.logDebug(`🔊 Voz seleccionada: ${this.spanishVoice.name} (${this.spanishVoice.lang})`);
                    } else {
                        this.logDebug('⚠️ No se encontró voz en español, usando voz por defecto');
                    }
                };

                if (this.synthesis.getVoices().length > 0) {
                    loadVoices();
                } else {
                    this.synthesis.onvoiceschanged = loadVoices;
                }
            }

            initEventListeners() {
                this.logDebug('🔧 Configurando event listeners...');

                // Test de micrófono
                this.testBtn.addEventListener('click', (e) => {
                    e.preventDefault();
                    this.logDebug('🔧 Test de micrófono iniciado por el usuario');
                    this.testMicrophone();
                });

                // Sistema de mantener presionado para hablar - MOUSE
                this.listenBtn.addEventListener('mousedown', (e) => {
                    e.preventDefault();
                    this.logDebug('🖱️ Mouse down - iniciando escucha');
                    this.startListening();
                });

                this.listenBtn.addEventListener('mouseup', (e) => {
                    e.preventDefault();
                    this.logDebug('🖱️ Mouse up - deteniendo escucha');
                    this.stopListening();
                });

                this.listenBtn.addEventListener('mouseleave', (e) => {
                    if (this.isListening) {
                        this.logDebug('🖱️ Mouse leave - deteniendo escucha');
                        this.stopListening();
                    }
                });

                // Sistema de mantener presionado para hablar - TOUCH
                this.listenBtn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.logDebug('👆 Touch start - iniciando escucha');
                    this.startListening();
                });

                this.listenBtn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    this.logDebug('👆 Touch end - deteniendo escucha');
                    this.stopListening();
                });

                this.listenBtn.addEventListener('touchcancel', (e) => {
                    e.preventDefault();
                    this.logDebug('👆 Touch cancel - deteniendo escucha');
                    this.stopListening();
                });

                // Control de volumen
                this.volumeSlider.addEventListener('input', (e) => {
                    this.volume = parseFloat(e.target.value);
                    this.volumeDisplay.textContent = Math.round(this.volume * 100) + '%';
                    this.logDebug(`🔊 Volumen cambiado a: ${Math.round(this.volume * 100)}%`);
                });

                // Solicitar permisos al primer clic
                document.addEventListener('click', () => {
                    this.requestPermissions();
                }, { once: true });

                this.logDebug('✅ Event listeners configurados');
            }

            async requestPermissions() {
                try {
                    this.logDebug('🔐 Solicitando permisos de micrófono...');
                    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                    stream.getTracks().forEach(track => track.stop());
                    this.logDebug('✅ Permisos de micrófono concedidos');
                    this.showSuccess('✅ Permisos de micrófono concedidos');
                } catch (error) {
                    this.logDebug(`❌ Error permisos: ${error.message}`);
                    this.showError('❌ Necesitas permitir el acceso al micrófono para usar el asistente');
                }
            }

            async testMicrophone() {
                this.updateStatus('🔧 Probando micrófono...');
                this.logDebug('🔧 Iniciando test de micrófono');
                
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                    this.logDebug('✅ Stream de audio obtenido');
                    this.showSuccess('✅ Micrófono funcionando correctamente');
                    this.speak('Perfecto. Tu micrófono está funcionando correctamente. Ahora puedes usar el botón de hablar.');
                    
                    stream.getTracks().forEach(track => track.stop());
                    this.logDebug('✅ Stream cerrado');
                    
                } catch (error) {
                    this.logDebug(`❌ Error en test: ${error.message}`);
                    let errorMsg = '';
                    switch(error.name) {
                        case 'NotAllowedError':
                            errorMsg = 'Permisos denegados. Permite el acceso al micrófono en la configuración del navegador.';
                            break;
                        case 'NotFoundError':
                            errorMsg = 'No se encontró micrófono. Verifica que esté conectado.';
                            break;
                        default:
                            errorMsg = error.message;
                    }
                    this.showError(`❌ Error: ${errorMsg}`);
                }
            }

            startListening() {
                if (!this.recognition || this.isListening || this.isSpeaking) {
                    this.logDebug('⚠️ No se puede iniciar escucha: recognition=' + !!this.recognition + ', isListening=' + this.isListening + ', isSpeaking=' + this.isSpeaking);
                    return;
                }
                
                try {
                    this.recognition.start();
                    this.logDebug('🎤 Iniciando escucha...');
                } catch (error) {
                    this.logDebug(`❌ Error al iniciar: ${error.message}`);
                    if (error.name === 'InvalidStateError') {
                        this.logDebug('⚠️ Reconocimiento ya está activo');
                    } else {
                        this.showError('❌ Error al iniciar el reconocimiento');
                    }
                }
            }

            stopListening() {
                if (!this.recognition || !this.isListening) {
                    this.logDebug('⚠️ No se puede parar escucha: recognition=' + !!this.recognition + ', isListening=' + this.isListening);
                    return;
                }
                
                try {
                    this.recognition.stop();
                    this.logDebug('🛑 Deteniendo escucha...');
                } catch (error) {
                    this.logDebug(`❌ Error al detener: ${error.message}`);
                }
            }

            processUserInput(input) {
                this.logDebug(`🤔 Procesando entrada del usuario: "${input}"`);
                this.updateStatus('🤔 Procesando tu pregunta...');
                
                const response = this.generateResponse(input.toLowerCase());
                this.displayResponse(response);
                this.speak(response);
            }

            generateResponse(input) {
                this.logDebug(`🧠 Generando respuesta para: "${input}"`);
                
                // Respuestas simples
                if (input.includes('hora')) {
                    return `Son las ${new Date().toLocaleTimeString('es-ES', {hour: '2-digit', minute: '2-digit'})}`;
                }
                
                if (input.includes('fecha') || input.includes('día')) {
                    return `Hoy es ${new Date().toLocaleDateString('es-ES', { 
                        weekday: 'long', day: 'numeric', month: 'long' 
                    })}`;
                }
                
                if (input.includes('chiste')) {
                    const chistes = [
                        "¿Por qué los pájaros vuelan hacia el sur en invierno? Porque caminando tardarían mucho más.",
                        "¿Qué le dice un semáforo a otro? No me mires que me estoy cambiando.",
                        "¿Cómo se llama el campeón de buceo japonés? Tokofondo."
                    ];
                    return chistes[Math.floor(Math.random() * chistes.length)];
                }
                
                if (input.includes('cómo estás') || input.includes('qué tal')) {
                    return "Estoy muy bien, gracias por preguntar. Listo para ayudarte con lo que necesites.";
                }
                
                if (input.includes('clima') || input.includes('tiempo')) {
                    return "No tengo acceso a datos meteorológicos en tiempo real, pero espero que tengas un día soleado.";
                }
                
                if (input.includes('hola') || input.includes('buenos días') || input.includes('buenas tardes')) {
                    return "¡Hola! Un gusto saludarte. ¿En qué puedo ayudarte hoy?";
                }
                
                if (input.includes('gracias')) {
                    return "¡De nada! Es un placer poder ayudarte.";
                }
                
                if (input.includes('adiós') || input.includes('hasta luego')) {
                    return "¡Hasta luego! Que tengas un excelente día.";
                }
                
                // Respuesta por defecto
                return `Escuché que dijiste "${input}". Aunque soy una versión simple, puedo ayudarte con la hora, chistes, saludos y más. ¿Qué más te gustaría saber?`;
            }

            displayResponse(response) {
                this.responseText.textContent = response;
                this.logDebug(`💬 Mostrando respuesta: "${response}"`);
            }

            speak(text) {
                if (this.synthesis.speaking) {
                    this.synthesis.cancel();
                }

                this.logDebug(`🗣️ Preparando para hablar: "${text}"`);

                const utterance = new SpeechSynthesisUtterance(text);
                if (this.spanishVoice) {
                    utterance.voice = this.spanishVoice;
                }
                utterance.volume = this.volume;
                utterance.rate = 0.9;
                utterance.pitch = 1;

                utterance.onstart = () => {
                    this.isSpeaking = true;
                    this.claudeIndicator.classList.add('speaking');
                    this.claudeIndicator.textContent = '🗣️';
                    this.updateStatus('🗣️ Claude está hablando...');
                    this.logDebug('🗣️ Síntesis de voz iniciada');
                };

                utterance.onend = () => {
                    this.isSpeaking = false;
                    this.claudeIndicator.classList.remove('speaking');
                    this.claudeIndicator.textContent = '🎤';
                    this.updateStatus('✅ Listo para la siguiente pregunta');
                    this.logDebug('✅ Síntesis de voz completada');
                };

                utterance.onerror = (event) => {
                    this.logDebug(`❌ Error en síntesis: ${event.error}`);
                    this.isSpeaking = false;
                    this.claudeIndicator.classList.remove('speaking');
                    this.claudeIndicator.textContent = '🎤';
                    this.updateStatus('❌ Error al reproducir audio');
                };

                this.synthesis.speak(utterance);
            }

            updateStatus(message) {
                this.status.innerHTML = message;
            }

            showError(message) {
                this.status.innerHTML = `<div class="error">${message}</div>`;
            }

            showSuccess(message) {
                this.status.innerHTML = `<div class="success">${message}</div>`;
            }

            logDebug(message) {
                const timestamp = new Date().toLocaleTimeString();
                const logEntry = `[${timestamp}] ${message}`;
                console.log(logEntry);
                
                this.debugContent.innerHTML = logEntry + '<br>' + this.debugContent.innerHTML;
                
                // Mantener solo las últimas 15 entradas
                const lines = this.debugContent.innerHTML.split('<br>');
                if (lines.length > 15) {
                    this.debugContent.innerHTML = lines.slice(0, 15).join('<br>');
                }
            }
        }

        // Inicializar cuando la página esté lista
        document.addEventListener('DOMContentLoaded', () => {
            new SimpleClaudeAssistant();
        });
    </script>
</body>
</html>
