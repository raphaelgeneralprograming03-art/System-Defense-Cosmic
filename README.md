<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matriz de Evolução Cósmica - Tríade de Naves</title>
    <style>
        :root {
            --bg-dark: #030308;
            --panel-bg: rgba(8, 8, 20, 0.9);
            --neon-blue: #00d2ff;
            --neon-green: #2eff70;
            --neon-purple: #b500ff;
            --neon-red: #ff0055;
            --neon-amber: #ffaa00;
            --text-light: #d8d8f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-light);
            font-family: 'Courier New', Courier, monospace;
            overflow: hidden;
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        header {
            background: linear-gradient(180deg, rgba(181,0,255,0.1) 0%, transparent 100%);
            border-bottom: 1px solid rgba(181, 0, 255, 0.3);
            padding: 12px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h1 {
            font-size: 1.2rem;
            color: var(--neon-purple);
            text-shadow: 0 0 8px rgba(181, 0, 255, 0.5);
        }

        .main-container {
            display: flex;
            flex: 1;
            height: calc(100vh - 50px);
        }

        /* Painel Lateral */
        .control-panel {
            width: 350px;
            background: var(--panel-bg);
            border-right: 1px solid rgba(0, 210, 255, 0.2);
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow-y: auto;
            backdrop-filter: blur(12px);
        }

        .section-title {
            font-size: 0.9rem;
            color: var(--neon-blue);
            border-bottom: 1px dashed rgba(0, 210, 255, 0.3);
            padding-bottom: 4px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .grid-selector {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .btn-opt {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #a0a0c0;
            padding: 10px;
            text-align: left;
            cursor: pointer;
            border-radius: 4px;
            font-family: inherit;
            font-size: 0.8rem;
            transition: all 0.2s ease;
        }

        .btn-opt:hover {
            background: rgba(0, 210, 255, 0.08);
            border-color: var(--neon-blue);
        }

        .btn-opt.active {
            color: #fff;
            box-shadow: 0 0 10px rgba(0, 210, 255, 0.2);
        }

        /* Cores dinâmicas para os botões ativos dependendo do estado */
        .btn-danger.active { border-color: var(--neon-red); background: rgba(255, 0, 85, 0.1); }
        .btn-method.active { border-color: var(--neon-green); background: rgba(46, 255, 112, 0.1); }

        /* Monitor de Status */
        .status-display {
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(255, 255, 255, 0.05);
            padding: 10px;
            border-radius: 4px;
            font-size: 0.75rem;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .status-row {
            display: flex;
            justify-content: space-between;
        }

        .status-val {
            font-weight: bold;
        }

        /* Área de Simulação */
        .simulation-area {
            flex: 1;
            position: relative;
            background: #010103;
        }

        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        /* Interface HUD inferior */
        .hud-bottom {
            position: absolute;
            bottom: 15px;
            left: 15px;
            right: 15px;
            background: rgba(3, 3, 6, 0.9);
            border: 1px solid rgba(0, 210, 255, 0.2);
            padding: 12px;
            border-radius: 4px;
            pointer-events: none;
        }

        .hud-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .hud-desc {
            font-size: 0.75rem;
            line-height: 1.4;
            color: #a5a5c5;
        }
    </style>
</head>
<body>

    <header>
        <h1>MATRIZ ANALÍTICA: ANÁLISE DE SOBREVIVÊNCIA DA TRÍADE</h1>
        <div id="global-safety" style="font-size: 0.85rem; font-weight: bold; color: var(--neon-red);">SEGURANÇA: VULNERÁVEL</div>
    </header>

    <div class="main-container">
        <!-- Painel de Configurações -->
        <div class="control-panel">
            <div class="section-title">1. Selecionar Perigo Original</div>
            <div class="grid-selector">
                <button class="btn-opt btn-danger active" onclick="setDanger('hawking')">A. Radiação Hawking / Térmica</button>
                <button class="btn-opt btn-danger" onclick="setDanger('tide')">B. Forças de Maré (Espaguetificação)</button>
                <button class="btn-opt btn-danger" onclick="setDanger('collapse')">C. Instabilidade e Colapso Crônico</button>
            </div>

            <div class="section-title" style="margin-top: 10px;">2. Configuração de Metodologia</div>
            <div class="grid-selector">
                <button class="btn-opt btn-method active" onclick="setMethod(1)">MÉTODO 1: Tríade Convencional (Estática)</button>
                <button class="btn-opt btn-method" onclick="setMethod(2)">MÉTODO 2: Tríade + Velocidade da Luz (99.99991%c)</button>
                <button class="btn-opt btn-method" onclick="setMethod(3)">MÉTODO 3: Tríade + V.Luz + Matéria Negativa</button>
                <button class="btn-opt btn-method" onclick="setMethod(4)">MÉTODO 4: Fusão Total (Tríades Completas)</button>
            </div>

            <div class="section-title" style="margin-top: 10px;">Monitor de Parâmetros</div>
            <div class="status-display">
                <div class="status-row"><span>Velocidade Angular:</span><span id="p-vel" class="status-val">0 km/s</span></div>
                <div class="status-row"><span>Massa Relativística:</span><span id="p-mass" class="status-val">1.0x (Estática)</span></div>
                <div class="status-row"><span>Fluxo Matéria Negativa:</span><span id="p-neg" class="status-val">0.00 kg/s</span></div>
                <div class="status-row"><span>Captura Matéria Escura:</span><span id="p-dark" class="status-val">INATIVA</span></div>
                <div class="status-row"><span>Estabilidade do Espaço-Tempo:</span><span id="p-stab" class="status-val" style="color:var(--neon-red)">CRÍTICA</span></div>
            </div>
        </div>

        <!-- Área de Renderização -->
        <div class="simulation-area">
            <canvas id="evolCanvas"></canvas>
            
            <div class="hud-bottom">
                <div class="hud-header">
                    <span id="hud-title-text" style="color: var(--neon-blue);">MÉTODO 1 VS RADIAÇÃO HAWKING</span>
                    <span id="hud-status-badge" style="color: var(--neon-red);">STATUS: DESTRUIÇÃO DA FROTA</span>
                </div>
                <div id="hud-body-text" class="hud-desc">Carregando dados matemáticos...</div>
            </div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('evolCanvas');
        const ctx = canvas.getContext('2d');

        function resize() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resize);
        resize();

        // Estados
        let activeDanger = 'hawking';
        let activeMethod = 1;
        let tick = 0;

        // Base de Dados de Simulação Cruzada Matrix[Perigo][Método]
        const simulationMatrix = {
            hawking: {
                name: "Radiação Hawking / Térmica Extrema",
                methods: {
                    1: {
                        title: "MÉTODO 1: Tríade Convencional (Sem V.Luz / Sem Buraco)",
                        status: "DESTRUIÇÃO IMINENTE",
                        color: "var(--neon-red)",
                        text: "As três naves operam em posições fixas no espaço normal. Sem a dilatação temporal ou campos exóticos, o calor térmico e a emissão de partículas subatômicas evaporam instantaneamente as blindagens mecânicas das naves A, B e da Sonda Central. Falha estrutural catastrófica.",
                        stab: "0.01% - Caótica", vel: "0 km/s", mass: "1.0x", neg: "0.00 kg/s", dark: "INATIVA"
                    },
                    2: {
                        title: "MÉTODO 2: Tríade em Rotação Relativística (99.99991% c)",
                        status: "DANOS GRAVES / INSUFICIENTE",
                        color: "var(--neon-amber)",
                        text: "A rotação a 99.99991%c faz o tempo passar 745 vezes mais devagar para as naves A e B, diminuindo a taxa de absorção de radiação. Contudo, a Sonda Central estacionária no centro acumula o calor comprimido (Blueshift), sofrendo derretimento de seus sistemas eletrônicos internos.",
                        stab: "14.2% - Instável", vel: "299.792,2 km/s", mass: "745.35x", neg: "0.00 kg/s", dark: "INATIVA"
                    },
                    3: {
                        title: "MÉTODO 3: Tríade + Velocidade da Luz + Matéria Negativa",
                        status: "SUPORTADO COM RISCO",
                        color: "var(--neon-blue)",
