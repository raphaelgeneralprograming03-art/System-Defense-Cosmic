<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel Tríade - Sistema de Defesa Cósmico</title>
    <style>
        :root {
            --bg-color: #020206;
            --panel-bg: rgba(6, 6, 20, 0.92);
            --neon-blue: #00f3ff;
            --neon-green: #39ff14;
            --neon-purple: #bd00ff;
            --neon-red: #ff0055;
            --text-main: #e0e0ff;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: 'Courier New', Courier, monospace;
            overflow: hidden;
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        header {
            background: linear-gradient(180deg, rgba(0,243,255,0.15) 0%, transparent 100%);
            border-bottom: 1px solid rgba(0, 243, 255, 0.3);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 30;
        }

        header h1 {
            font-size: 1.3rem;
            color: var(--neon-blue);
            text-shadow: 0 0 10px rgba(0, 243, 255, 0.5);
        }

        .system-status {
            font-size: 0.85rem;
            color: var(--neon-green);
        }

        .main-container {
            display: flex;
            flex: 1;
            height: calc(100vh - 60px);
            position: relative;
        }

        /* Painel Lateral */
        .control-panel {
            width: 330px;
            background: var(--panel-bg);
            border-right: 1px solid rgba(0, 243, 255, 0.25);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow-y: auto;
            backdrop-filter: blur(15px);
            z-index: 30;
        }

        .section-title {
            font-size: 0.95rem;
            color: var(--neon-blue);
            border-bottom: 1px dashed rgba(0, 243, 255, 0.4);
            padding-bottom: 5px;
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .danger-btn {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(0, 243, 255, 0.25);
            color: #b0b0d0;
            padding: 12px;
            text-align: left;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s ease;
            font-family: inherit;
            font-size: 0.8rem;
        }

        .danger-btn:hover {
            background: rgba(0, 243, 255, 0.08);
            border-color: var(--neon-blue);
        }

        .danger-btn.active {
            background: rgba(189, 0, 255, 0.15);
            border-color: var(--neon-purple);
            box-shadow: 0 0 12px rgba(189, 0, 255, 0.4);
            color: #fff;
            font-weight: bold;
        }

        .telemetry-box {
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid rgba(0, 243, 255, 0.15);
            padding: 12px;
            border-radius: 4px;
            font-size: 0.75rem;
            line-height: 1.5;
        }

        .telemetry-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
        }

        .telemetry-value {
            color: var(--neon-blue);
            font-weight: bold;
        }

        /* Área do Espaço */
        .simulation-area {
            flex: 1;
            position: relative;
            background: #020208;
            height: 100%;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: block;
            z-index: 10;
        }

        /* Interface Sobreposta */
        .hud-overlay {
            position: absolute;
            bottom: 20px;
            left: 20px;
            right: 20px;
            background: rgba(4, 4, 12, 0.93);
            border: 1px solid rgba(189, 0, 255, 0.35);
            padding: 15px;
            border-radius: 4px;
            pointer-events: none;
            backdrop-filter: blur(8px);
            z-index: 30;
        }

        .hud-title {
            color: var(--neon-purple);
            font-size: 0.95rem;
            margin-bottom: 6px;
            text-transform: uppercase;
        }

        .hud-desc {
            font-size: 0.8rem;
            color: #b5b5d5;
            line-height: 1.5;
        }

        .scale-badge {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 0, 85, 0.15);
            border: 1px solid var(--neon-red);
            color: var(--neon-red);
            padding: 5px 10px;
            font-size: 0.7rem;
            border-radius: 3px;
            text-transform: uppercase;
            z-index: 30;
        }
    </style>
</head>
<body>

    <header>
        <h1>SISTEMA DE DEFESA DA TRÍADE (MHD / S.A.M.S / SROS)</h1>
        <div class="system-status">COLETA DE COMBUSTÍVEL DO VÁCUO (H & He): ATIVA</div>
    </header>

    <div class="main-container">
        <!-- Painel Lateral -->
        <div class="control-panel">
            <div class="section-title">Ameaças Espaciais</div>
            <button class="danger-btn active" onclick="switchDanger('supernova')">1. Supernova Cósmica</button>
            <button class="danger-btn" onclick="switchDanger('grb')">2. Surto de Raios Gama (GRB)</button>
            <button class="danger-btn" onclick="switchDanger('blackhole')">3. Buraco Negro Supermassivo</button>
            <button class="danger-btn" onclick="switchDanger('gravwave')">4. Onda Gravitacional Extrema</button>
            <button class="danger-btn" onclick="switchDanger('vacuum')">5. Decaimento do Vácuo Quântico</button>
            <button class="danger-btn" onclick="switchDanger('blueshift')">6. Radiação com Desvio Azul</button>

            <div class="section-title" style="margin-top: 10px;">Telemetria Analítica</div>
            <div class="telemetry-box">
                <div class="telemetry-row"><span>Velocidade Giro:</span><span class="telemetry-value">0.9999991 c</span></div>
                <div class="telemetry-row"><span>Fator Lorentz:</span><span class="telemetry-value">745.35x</span></div>
                <div class="telemetry-row"><span>Matéria Negativa:</span><span id="tel-neg" class="telemetry-value">--</span></div>
                <div class="telemetry-row"><span>Matéria Escura:</span><span id="tel-dark" class="telemetry-value">--</span></div>
                <div class="telemetry-row"><span>Escudos MHD:</span><span class="telemetry-value" style="color:var(--neon-green)">ESTÁVEL</span></div>
                <div class="telemetry-row"><span>S.A.M.S / SROS:</span><span class="telemetry-value" style="color:var(--neon-green)">SINCRONIZADO</span></div>
            </div>
        </div>

        <!-- Área Espacial -->
        <div class="simulation-area">
            <div id="scale-indicator" class="scale-badge">Carregando...</div>
            <canvas id="simCanvas"></canvas>
            
            <div class="hud-overlay">
                <div id="hud-name" class="hud-title">Carregando...</div>
                <div id="hud-text" class="hud-desc">Iniciando alinhamento dos escudos quânticos...</div>
            </div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Garantir o dimensionamento correto antes de qualquer desenho
        canvas.width = canvas.parentElement.clientWidth || 800;
        canvas.height = canvas.parentElement.clientHeight || 600;

        window.addEventListener('resize', () => {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        });

        let currentDanger = 'supernova';
        let time = 0;

        // Gerar coordenadas de estrelas
        const stars = [];
        for(let i = 0; i < 120; i++) {
            stars.push({ x: Math.random(), y: Math.random(), size: Math.random() * 1.5 + 0.5 });
        }

        const dangerData = {
            supernova: {
                name: "1. Supernova Cósmica (Magnitude: ExaJoules Estelares)",
                text: "A onda de energia e plasma da estrela colide contra o escudo. A Tríade canaliza Matéria Escura pelas bordas do portal via SROS, gerando uma lente gravitacional macroscópica que curva e desvia a destruição ao redor do planeta protegido.",
                scale: "Magnitude: ExaJoules", neg: "4.5 x 10^22 kg/s", dark: "9.8 x 10^26 kg/s"
            },
            grb: {
                name: "2. Surto de Raios Gama - GRB (Magnitude: Feixe Relativístico)",
                text: "O feixe linear de alta energia atinge o perímetro. O sistema de deflexão do MHD aciona os jatos de Matéria Negativa, gerando uma repulsão métrica (antigravidade). O feixe de fótons é refratado e expelido para o vácuo.",
                scale: "Magnitude: YottaWatts", neg: "8.9 x 10^24 kg/s", dark: "1.2 x 10^22 kg/s"
            },
            blackhole: {
                name: "3. Buraco Negro Supermassivo (Magnitude: Bilhões de Massas Solares)",
                text: "A atração gravitacional esmagadora tenta reter o sistema. A Sonda Central, amparada pelo amortecimento inercial do S.A.M.S, projeta um fluxo de massa exótica criando o canal do Buraco de Minhoca para evasão imediata.",
                scale: "Magnitude: Escala Métrica", neg: "7.7 x 10^27 kg/s", dark: "3.4 x 10^25 kg/s"
            },
            gravwave: {
