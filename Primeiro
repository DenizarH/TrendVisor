<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TrendVisor</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif; /* Fonte principal para o texto */
            background-color: #111827; /* gray-900 */
        }
        .card {
            background-color: #1f2937; /* gray-800 */
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            border: 1px solid #374151; /* gray-700 */
        }
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #38bdf8; /* sky-400 */
            animation: spin 1s ease infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        #imageContainer {
            position: relative;
            max-width: 100%;
            margin: 1rem auto;
            background-color: #374151; /* gray-700 */
            border-radius: 0.5rem;
            overflow: hidden;
        }
        #imagePreview, #annotationCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: block;
        }
        #imagePreview {
            position: relative;
            z-index: 1;
        }
        #annotationCanvas {
            z-index: 2;
            pointer-events: none;
        }
        .upload-area {
            border-color: #4b5563; /* gray-600 */
        }
        .upload-area.disabled {
            opacity: 0.6;
            cursor: not-allowed;
            background-color: #111827; /* gray-900 */
        }
        .analysis-section h3 {
            font-size: 1.125rem;
            font-weight: 700;
            border-bottom: 2px solid #374151; /* gray-700 */
            padding-bottom: 0.5rem;
            margin-bottom: 0.75rem;
        }
        .analysis-section p, .analysis-section li, .analysis-section .advice {
            font-size: 0.875rem;
            color: #d1d5db; /* gray-300 */
            line-height: 1.6;
        }
        .analysis-section .advice {
            margin-top: 1rem;
            padding: 0.75rem;
            background-color: #1e293b; /* slate-800 */
            border-left: 4px solid #38bdf8; /* sky-400 */
            border-radius: 0.25rem;
        }
        .analysis-section .advice strong {
            color: #7dd3fc; /* sky-300 */
        }
        .analysis-section ul {
            list-style-type: disc;
            padding-left: 1.5rem;
        }
        .sentiment-balance-section .arrow-display {
            font-size: 3rem;
            line-height: 1;
        }
        .sentiment-balance-section .percent-display {
            font-size: 1.5rem;
            font-weight: 800; /* extrabold */
        }
        .info-message {
            background-color: #164e63; /* cyan-800 */
            color: #a5f3fc; /* cyan-200 */
            padding: 1rem;
            border-radius: 0.5rem;
            margin-bottom: 1.5rem;
            text-align: center;
        }
    </style>
</head>
<body class="bg-gray-900">
    <div class="container max-w-4xl mx-auto p-4">
        <header class="text-center mb-8 pt-6">
            <h1 class="text-5xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-sky-400 to-fuchsia-500 inline-flex items-center gap-x-4">
                TrendVisor
                <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" viewBox="0 0 24 24" fill="none" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M3 17l6-6 4 4 8-8" stroke="#4ade80" />
                    <path d="M17 3h4v4" stroke="#4ade80" />
                    <path d="M3 3v18h18" stroke="#4b5563" />
                </svg>
            </h1>
            <p class="text-gray-400 mt-3">Análise técnica e visual com IA para seus gráficos.</p>
        </header>

        <!-- Seção de Upload -->
        <div class="card">
            <input type="file" id="imageUpload" accept="image/*" class="hidden">
            <div id="uploadArea" class="border-2 border-dashed rounded-lg p-8 text-center cursor-pointer hover:bg-gray-700/50 transition-colors">
                <svg class="mx-auto h-12 w-12 text-gray-500" stroke="currentColor" fill="none" viewBox="0 0 48 48" aria-hidden="true">
                    <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                </svg>
                <p class="mt-2 text-sm text-gray-400">
                    <span class="font-semibold text-sky-400 hover:text-sky-300">Clique para carregar uma imagem</span>
                </p>
                <p class="text-xs text-gray-500">PNG, JPG, etc.</p>
            </div>
            <button id="analyzeButton" class="mt-6 w-full bg-sky-600 hover:bg-sky-700 text-white font-bold py-3 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500 focus:ring-offset-2 disabled:opacity-50 transition-all shadow-lg hover:shadow-sky-400/50" disabled>
                Fazer Análise Técnica Completa
            </button>
        </div>

        <div id="infoMessage" class="hidden info-message"></div>
        <div id="loadingIndicator" class="hidden text-center py-5">
            <div class="spinner"></div>
            <p class="text-gray-400">Analisando... A IA está processando o gráfico completo.</p>
        </div>
        <div id="errorMessage" class="hidden bg-red-800 text-red-200 p-4 rounded-lg"></div>

        <!-- Área de Resultados -->
        <div id="resultsArea" class="hidden">
            <!-- Gráfico com Anotações -->
            <div class="card">
                <h2 class="text-2xl font-bold text-gray-100 mb-4">Gráfico Analisado</h2>
                <div id="imageContainer">
                    <img id="imagePreview" src="#" alt="Gráfico do usuário com anotações">
                    <canvas id="annotationCanvas"></canvas>
                </div>
            </div>

            <!-- Análise Técnica em Texto -->
            <div class="card">
                <h2 class="text-2xl font-bold text-gray-100 mb-4">Análise Técnica Detalhada</h2>
                <div id="analysisContent" class="space-y-6">
                    <!-- Seções de análise serão inseridas aqui -->
                </div>
            </div>
        </div>
    </div>

    <script>
        const imageUpload = document.getElementById('imageUpload');
        const uploadArea = document.getElementById('uploadArea');
        const analyzeButton = document.getElementById('analyzeButton');
        const loadingIndicator = document.getElementById('loadingIndicator');
        const errorMessage = document.getElementById('errorMessage');
        const infoMessage = document.getElementById('infoMessage');
        const resultsArea = document.getElementById('resultsArea');
        const imageContainer = document.getElementById('imageContainer');
        const imagePreview = document.getElementById('imagePreview');
        const annotationCanvas = document.getElementById('annotationCanvas');
        const analysisContent = document.getElementById('analysisContent');

        let imageDataBase64 = null;
        let currentImageHash = null;
        let lastAnalysisData = null;

        uploadArea.addEventListener('click', () => {
            if (!uploadArea.classList.contains('disabled')) {
                imageUpload.click();
            }
        });
        imageUpload.addEventListener('change', handleFile);

        async function sha256(str) {
            const textAsBuffer = new TextEncoder().encode(str);
            const hashBuffer = await crypto.subtle.digest('SHA-256', textAsBuffer);
            const hashArray = Array.from(new Uint8Array(hashBuffer));
            return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        }

        async function handleFile(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = async (e) => {
                imageDataBase64 = e.target.result.split(',')[1];
                currentImageHash = await sha256(imageDataBase64);
                
                imagePreview.src = e.target.result;
                imagePreview.onload = () => {
                    annotationCanvas.width = imagePreview.offsetWidth;
                    annotationCanvas.height = imagePreview.offsetHeight;
                };
                
                analyzeButton.disabled = false;
                resultsArea.classList.add('hidden');
                errorMessage.classList.add('hidden');
                infoMessage.classList.add('hidden');
                
                const ctx = annotationCanvas.getContext('2d');
                ctx.clearRect(0, 0, annotationCanvas.width, annotationCanvas.height);
            };
            reader.readAsDataURL(file);
        }

        analyzeButton.addEventListener('click', async () => {
            if (!imageDataBase64 || !currentImageHash) {
                showError("Por favor, carregue uma imagem primeiro.");
                return;
            }
            
            loadingIndicator.classList.remove('hidden');
            uploadArea.classList.add('disabled');
            resultsArea.classList.add('hidden');
            errorMessage.classList.add('hidden');
            infoMessage.classList.add('hidden');
            analyzeButton.disabled = true;

            const cachedResult = localStorage.getItem(currentImageHash);
            if (cachedResult) {
                console.log("Resultado encontrado no cache!");
                lastAnalysisData = JSON.parse(cachedResult);
                displayResults(lastAnalysisData);
                showInfo("Análise carregada do histórico. O resultado é o mesmo da última vez que esta imagem foi analisada.");
                loadingIndicator.classList.add('hidden');
                analyzeButton.disabled = false;
                uploadArea.classList.remove('disabled');
                return; 
            }
            
            try {
                const prompt = `Aja como um analista técnico de mercado financeiro. Analise a imagem do gráfico de candlestick fornecida e retorne uma análise completa estritamente no formato JSON abaixo.

                Análise Solicitada:
                1.  **Analise de Tendência**: Descreva a tendência geral (alta, baixa, lateral).
                2.  **Padrões Gráficos (Formações)**: Analise a forma geral do gráfico para identificar padrões maiores (Ombro-Cabeça-Ombro, Topo/Fundo Duplo, Triângulos, Bandeiras, etc.). Se um padrão for identificado, descreva-o. Se nenhum padrão gráfico maior for aparente, afirme 'Sem padrão gráfico identificado'.
                3.  **Suporte e Resistência**: Identifique os níveis de suporte e resistência mais óbvios.
                4.  **Análise de Padrão de Candlestick Recente**: Foque nos últimos 6 candles. Identifique se existe algum dos seguintes padrões: Martelo, Estrela Cadente, Engolfo de Alta, Engolfo de Baixa, Doji, Bebê Abandonado, Piercing Pattern, Três Soldados Brancos, Três Corvos Negros, Harami de Alta, Harami de Baixa, Doji de Pernas Longas, Topo/Fundo Abandonado.
                    - Se encontrar um padrão, forneça seu nome, uma explicação detalhada de como ele se forma e o que sinaliza, e um "aconselhamento" educacional sobre como um trader poderia interpretar o sinal (ex: buscar confirmação de volume), sempre com a ressalva de que não é recomendação financeira.
                    - Se nenhum desses padrões for encontrado, informe 'Nenhum padrão de candlestick relevante identificado nos últimos 6 candles'.
                5.  **Localização para Anotação**: Se um padrão de candlestick for encontrado no passo 4, descreva sua localização para eu desenhar uma seta usando coordenadas relativas (X de 0 a 100, Y de 0 a 100).
                6.  **Projeção Especulativa**: Forneça uma projeção especulativa com uma faixa percentual e justificativa, deixando claro que é educacional.
                7.  **Balanço de Probabilidade**: Com base em toda a análise, estime a probabilidade de um movimento de alta versus um de baixa em porcentagens que somam 100.

                Formato JSON de Resposta Obrigatório:
                {
                  "technicalAnalysis": {
                    "trend": { "title": "Análise de Tendência", "content": "..." },
                    "chartPattern": { "title": "Padrões Gráficos (Formações)", "content": "..." },
                    "supportResistance": { "title": "Suporte e Resistência", "content": "..." },
                    "priceProjection": { "title": "Projeção Especulativa de Preço", "content": "..." },
                    "sentimentProbability": { "title": "Balanço de Probabilidade (Alta vs. Baixa)", "bullish": 0, "bearish": 0 }
                  },
                  "candlePatternAnalysis": {
                    "title": "Análise de Padrão de Candlestick Recente",
                    "found": false,
                    "name": null,
                    "explanation": "Explicação detalhada... ou 'Nenhum padrão...'",
                    "advice": "Aconselhamento educacional... ou null",
                    "implication": "'alta', 'baixa', 'indecisão' ou null",
                    "coordinates": { "x": 0, "y": 0 }
                  }
                }`;

                const payload = {
                    contents: [{
                        parts: [
                            { text: prompt },
                            { inlineData: { mimeType: "image/png", data: imageDataBase64 } }
                        ]
                    }],
                    generationConfig: {
                        responseMimeType: "application/json",
                    }
                };

                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) throw new Error(`Erro na API: ${response.statusText}`);

                const result = await response.json();
                
                if (!result.candidates || !result.candidates[0].content.parts[0].text) {
                    throw new Error("Resposta da IA inválida ou vazia.");
                }
                
                let jsonText = result.candidates[0].content.parts[0].text;
                if (jsonText.startsWith("```json")) {
                    jsonText = jsonText.substring(7, jsonText.length - 3).trim();
                } else if (jsonText.startsWith("```")) {
                    jsonText = jsonText.substring(3, jsonText.length - 3).trim();
                }

                lastAnalysisData = JSON.parse(jsonText);
                
                localStorage.setItem(currentImageHash, JSON.stringify(lastAnalysisData));
                console.log("Resultado salvo no cache para o hash:", currentImageHash);

                displayResults(lastAnalysisData);

            } catch (error) {
                console.error("Erro durante a análise:", error);
                showError(`Falha na análise: ${error.message}. Tente novamente com uma imagem mais clara.`);
            } finally {
                loadingIndicator.classList.add('hidden');
                analyzeButton.disabled = false;
                uploadArea.classList.remove('disabled');
            }
        });

        function displayResults(data) {
            analysisContent.innerHTML = '';
            const { technicalAnalysis, candlePatternAnalysis } = data;
            const titleColors = {
                trend: "text-cyan-300", chartPattern: "text-purple-400",
                supportResistance: "text-amber-400", candlePatternAnalysis: "text-blue-400",
                priceProjection: "text-indigo-400", sentimentProbability: "text-fuchsia-400"
            };

            const sectionOrder = ["trend", "chartPattern", "supportResistance", "candlePatternAnalysis", "priceProjection", "sentimentProbability"];
            
            const allSections = { ...technicalAnalysis, candlePatternAnalysis };

            sectionOrder.forEach(key => {
                if (allSections[key]) {
                    const sectionData = allSections[key];
                    const sectionDiv = document.createElement('div');
                    const h3Color = titleColors[key] || "text-gray-300";
                    sectionDiv.className = 'analysis-section';

                    if (key === 'sentimentProbability') {
                        sectionDiv.classList.add('sentiment-balance-section', 'text-center');
                        sectionDiv.innerHTML = `
                            <h3 class="${h3Color}">${sectionData.title}</h3>
                            <div class="flex justify-around items-center mt-4 p-4 bg-gray-900 rounded-lg">
                                <div class="text-green-400">
                                    <div class="arrow-display">▲</div>
                                    <p class="percent-display">${sectionData.bullish}% Alta</p>
                                </div>
                                <div class="text-red-400">
                                    <div class="arrow-display">▼</div>
                                    <p class="percent-display">${sectionData.bearish}% Baixa</p>
                                </div>
                            </div>`;
                    } else if (key === 'candlePatternAnalysis') {
                         sectionDiv.innerHTML = `
                            <h3 class="${h3Color}">${sectionData.title}</h3>
                            ${sectionData.found ? `<h4 class="text-gray-200"><strong>Padrão Encontrado:</strong> ${sectionData.name}</h4>` : ''}
                            <p>${sectionData.explanation.replace(/\n/g, '<br>')}</p>
                            ${sectionData.found && sectionData.advice ? `<div class="advice"><strong>Aconselhamento Educacional:</strong> ${sectionData.advice.replace(/\n/g, '<br>')}</div>` : ''}
                        `;
                    } else {
                        sectionDiv.innerHTML = `
                            <h3 class="${h3Color}">${sectionData.title}</h3>
                            <p>${sectionData.content.replace(/\n/g, '<br>')}</p>`;
                    }
                    analysisContent.appendChild(sectionDiv);
                }
            });

            const ctx = annotationCanvas.getContext('2d');
            ctx.clearRect(0, 0, annotationCanvas.width, annotationCanvas.height);
            if (candlePatternAnalysis && candlePatternAnalysis.found) {
                drawAnnotation(candlePatternAnalysis);
            }
            resultsArea.classList.remove('hidden');
        }

        function drawAnnotation(patternData) {
            const { name, implication, coordinates } = patternData;
            const canvasWidth = annotationCanvas.width;
            const canvasHeight = annotationCanvas.height;
            const x = (coordinates.x / 100) * canvasWidth;
            const y = (coordinates.y / 100) * canvasHeight;
            const ctx = annotationCanvas.getContext('2d');
            
            let arrowText, textY, arrowStartY, arrowEndY;
            const arrowColor = implication === 'alta' ? "#22c55e" : "#ef4444";
            const textColor = implication === 'alta' ? "#bbf7d0" : "#fecaca"; // light green/red
            
            if (implication === 'alta') {
                arrowText = "▲"; arrowStartY = y + 30; arrowEndY = y + 5; textY = y + 55;
            } else { // 'baixa' ou 'indecisão'
                arrowText = "▼"; arrowStartY = y - 30; arrowEndY = y - 5; textY = y - 35;
            }
            
            ctx.beginPath(); ctx.moveTo(x, arrowStartY); ctx.lineTo(x, arrowEndY);
            ctx.strokeStyle = arrowColor; ctx.lineWidth = 3; ctx.stroke();
            
            ctx.font = "bold 24px Arial"; ctx.fillStyle = arrowColor;
            ctx.textAlign = "center"; ctx.fillText(arrowText, x, implication === 'alta' ? arrowEndY : arrowEndY + 20);

            ctx.font = "bold 16px Arial"; ctx.textAlign = "center";
            const textMetrics = ctx.measureText(name);
            ctx.fillStyle = "rgba(0, 0, 0, 0.7)";
            ctx.fillRect(x - textMetrics.width / 2 - 8, textY - 18, textMetrics.width + 16, 24);
            ctx.fillStyle = textColor; ctx.fillText(name, x, textY);
        }

        function showError(message) {
            errorMessage.textContent = message;
            errorMessage.classList.remove('hidden');
        }
        
        function showInfo(message) {
            infoMessage.textContent = message;
            infoMessage.classList.remove('hidden');
        }

        window.addEventListener('resize', () => {
            if (imagePreview.src && !resultsArea.classList.contains('hidden') && lastAnalysisData) {
                annotationCanvas.width = imagePreview.offsetWidth;
                annotationCanvas.height = imagePreview.offsetHeight;
                const { candlePatternAnalysis } = lastAnalysisData;
                if (candlePatternAnalysis && candlePatternAnalysis.found) {
                    drawAnnotation(candlePatternAnalysis);
                }
            }
        });
    </script>
</body>
</html>
