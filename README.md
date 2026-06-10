<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Copa do Mundo 2026 — Mobile Dashboard</title>
    <style>
        /* ========== RESET & BASICS (MOBILE FIRST) ========== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent; /* Remove o flash azul ao tocar no iOS */
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background: #f4f7f9;
            color: #2C3E50;
            min-height: 100vh;
            padding-bottom: 60px; /* Espaço para não cobrir o conteúdo com o rodapé */
        }

        header {
            background: linear-gradient(135deg, #1B4965 0%, #0F2C42 100%);
            color: white;
            padding: 20px 15px;
            text-align: center;
            border-bottom-left-radius: 20px;
            border-bottom-right-radius: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        }

        header h1 {
            font-size: 1.6em;
            font-weight: 800;
            letter-spacing: 1px;
            margin-bottom: 4px;
        }

        header p {
            font-size: 0.85em;
            opacity: 0.85;
        }

        .container {
            padding: 15px;
            max-width: 600px; /* Focado estritamente no padrão de largura de celulares */
            margin: 0 auto;
        }

        /* ========== NAVEGAÇÃO INTERNA (TABS ESTILO APP) ========== */
        .tabs {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 5px;
            background: #e2e8f0;
            padding: 4px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .tab-button {
            padding: 10px 5px;
            background: transparent;
            border: none;
            color: #4A5568;
            font-weight: bold;
            font-size: 0.8em;
            cursor: pointer;
            border-radius: 8px;
            transition: all 0.2s ease;
            text-align: center;
            white-space: nowrap;
        }

        .tab-button.active {
            background: white;
            color: #1B4965;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: mobileFadeIn 0.25s ease-out;
        }

        @keyframes mobileFadeIn {
            from { opacity: 0; transform: translateY(5px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ========== GRID DASHBOARD (2 COLUNAS NO CELULAR) ========== */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .stat-card {
            background: white;
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            border: 1px solid #eef2f5;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .stat-card:active {
            background: #f8fafc; /* Feedback visual ao toque */
        }

        .stat-card.primary { border-top: 4px solid #1B4965; }
        .stat-card.accent { border-top: 4px solid #D62828; }
        .stat-card.success { border-top: 4px solid #06A77D; }
        .stat-card.warning { border-top: 4px solid #F77F00; }

        .stat-number {
            font-size: 1.8em;
            font-weight: bold;
            color: #1B4965;
            margin: 4px 0;
        }

        .stat-card.accent .stat-number { color: #D62828; }
        .stat-card.success .stat-number { color: #06A77D; }
        .stat-card.warning .stat-number { color: #F77F00; }

        .stat-label {
            font-size: 0.75em;
            color: #718096;
            text-transform: uppercase;
            font-weight: bold;
            letter-spacing: 0.5px;
        }

        .stat-hint {
            font-size: 0.65em;
            color: #a0aec0;
            margin-top: 2px;
        }

        /* ========== INFO SECTIONS ========== */
        .info-section {
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            margin-bottom: 15px;
        }

        .info-section h3 {
            color: #1B4965;
            font-size: 1em;
            margin-bottom: 12px;
            border-bottom: 2px solid #edf2f7;
            padding-bottom: 6px;
        }

        .info-list {
            list-style: none;
        }

        .info-list li {
            padding: 10px;
            background: #f8fafc;
            border-left: 3px solid #06A77D;
            border-radius: 4px;
            margin-bottom: 8px;
            font-size: 0.85em;
            line-height: 1.4;
        }

        .info-list li strong {
            color: #1B4965;
            display: block;
            margin-bottom: 2px;
        }

        /* ========== FILTROS E BUSCAS MOBILE ========== */
        .filter-wrapper {
            background: white;
            padding: 12px;
            border-radius: 12px;
            margin-bottom: 15px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .filter-wrapper input, 
        .filter-wrapper select {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid #cbd5e0;
            border-radius: 8px;
            font-size: 0.9em;
            outline: none;
            background-color: #f8fafc;
            -webkit-appearance: none; /* Uniformiza select no iOS */
        }

        .filter-wrapper select {
            background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'><path fill='%234A5568' d='M0 0l5 5 5-5z'/></svg>");
            background-repeat: no-repeat;
            background-position: right 12px center;
        }

        /* ========== JOGOS CARD-STYLE (EVITA QUEBRA DE TABELAS) ========== */
        .game-card {
            background: white;
            border-radius: 12px;
            padding: 12px;
            margin-bottom: 10px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.03);
            border-left: 4px solid #cbd5e0;
        }

        .game-card.brasil-match {
            border-left-color: #06A77D;
            background: #f0fdf4;
        }

        .game-card-meta {
            display: flex;
            justify-content: space-between;
            font-size: 0.75em;
            color: #718096;
            margin-bottom: 8px;
            border-bottom: 1px dashed #e2e8f0;
            padding-bottom: 4px;
        }

        .game-card-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
        }

        .game-card-team {
            flex: 1;
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.85em;
            font-weight: 600;
        }

        .game-card-team.right {
            justify-content: flex-end;
            text-align: right;
        }

        .game-card-score {
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .mobile-score-input {
            width: 38px;
            height: 34px;
            text-align: center;
            font-size: 0.95em;
            font-weight: bold;
            border: 1px solid #cbd5e0;
            border-radius: 6px;
            background: white;
            outline: none;
        }

        .mobile-score-input:focus {
            border-color: #1B4965;
            background: #f0fdf4;
        }

        .game-card-venue {
            font-size: 0.7em;
            color: #94a3b8;
            text-align: center;
            margin-top: 6px;
        }

        /* ========== CONTAINER DE GRUPOS INTERATIVOS ========== */
        .mobile-group-box {
            background: white;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            margin-bottom: 15px;
            overflow: hidden;
        }

        .mobile-group-header {
            background: #1B4965;
            color: white;
            padding: 12px 15px;
            font-size: 0.95em;
            font-weight: bold;
        }

        .mobile-group-body {
            padding: 12px;
        }

        .mobile-team-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px;
            background: #f8fafc;
            border-radius: 8px;
            margin-bottom: 6px;
            border-left: 3px solid #718096;
        }

        .mobile-team-item.destaque {
            border-left-color: #06A77D;
            background: #f0fdf4;
        }

        .mobile-team-left {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .mobile-team-name {
            font-size: 0.85em;
            font-weight: 600;
        }

        .mobile-team-desc {
            font-size: 0.7em;
            color: #718096;
        }

        .flag-icon {
            width: 22px;
            height: 15px;
            object-fit: cover;
            border-radius: 2px;
            box-shadow: 0 1px 2px rgba(0,0,0,0.1);
        }

        .flag-icon-large {
            width: 32px;
            height: 22px;
            object-fit: cover;
            border-radius: 3px;
        }

        /* ========== MODAIS INTEGRADOS (ESTILO SHEET ABAIXO) ========== */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 2000;
            justify-content: flex-end;
            flex-direction: column;
        }

        .modal {
            background: white;
            border-top-left-radius: 20px;
            border-top-right-radius: 20px;
            padding: 20px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 -4px 20px rgba(0,0,0,0.15);
            animation: slideUpMobile 0.25s ease-out;
        }

        @keyframes slideUpMobile {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #edf2f7;
            padding-bottom: 10px;
            margin-bottom: 15px;
        }

        .modal-header h3 {
            font-size: 1.1em;
            color: #1B4965;
        }

        .modal-close-btn {
            font-size: 1.5em;
            font-weight: bold;
            color: #a0aec0;
            border: none;
            background: transparent;
            cursor: pointer;
            padding: 0 5px;
        }

        .modal-grid-list {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .modal-list-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px;
            background: #f8fafc;
            border-radius: 8px;
            font-size: 0.8em;
            font-weight: 600;
        }

        .modal-block-item {
            background: #f8fafc;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 10px;
            border-left: 3px solid #1B4965;
            font-size: 0.8em;
            line-height: 1.4;
        }

        /* ========== FOOTER & TOAST NATIVOS ========== */
        footer {
            background: #0f2c42;
            color: #a0aec0;
            text-align: center;
            padding: 15px;
            font-size: 0.7em;
            position: fixed;
            bottom: 0; left: 0; width: 100%;
            z-index: 1000;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
        }

        .save-action-btn {
            background: #06A77D;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 12px;
            font-weight: bold;
            font-size: 0.9em;
            width: 100%;
            cursor: pointer;
            box-shadow: 0 2px 6px rgba(6,167,125,0.3);
            margin-top: 5px;
        }

        .toast {
            position: fixed;
            bottom: 70px;
            left: 50%;
            transform: translateX(-50%);
            background: #06A77D;
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 0.8em;
            font-weight: bold;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            z-index: 3000;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.2s ease;
        }
    </style>
</head>
<body>

    <header>
        <h1>⚽ COPA DO MUNDO 2026</h1>
        <p>Painel Interativo Otimizado para Celular</p>
    </header>

    <div class="container">
        <div class="tabs">
            <button class="tab-button active" onclick="switchMobileTab(event, 'tab-dashboard')">📊 Home</button>
            <button class="tab-button" onclick="switchMobileTab(event, 'tab-jogos')">📋 Tabela</button>
            <button class="tab-button" onclick="switchMobileTab(event, 'tab-grupos')">🗂️ Grupos</button>
        </div>

        <div id="tab-dashboard" class="tab-content active">
            <div class="dashboard-grid">
                <div class="stat-card primary" onclick="triggerMobileModal('selecoes')">
                    <span class="stat-label">Seleções</span>
                    <span class="stat-number">48</span>
                    <span class="stat-hint">Ver lista 🔍</span>
                </div>
                <div class="stat-card accent" onclick="switchMobileTab(null, 'tab-jogos')">
                    <span class="stat-label">Partidas</span>
                    <span class="stat-number">104</span>
                    <span class="stat-hint">Ir para tabela 📋</span>
                </div>
                <div class="stat-card success" onclick="triggerMobileModal('sedes')">
                    <span class="stat-label">Cidades-Sede</span>
                    <span class="stat-number">16</span>
                    <span class="stat-hint">Ver estádios 🏟️</span>
                </div>
                <div class="stat-card warning" onclick="triggerMobileModal('paises')">
                    <span class="stat-label">Anfitriões</span>
                    <span class="stat-number">3</span>
                    <span class="stat-hint">Ver detalhes 🌎</span>
                </div>
            </div>

            <div class="info-section">
                <h3>Informações Gerais</h3>
                <ul class="info-list">
                    <li><strong>📅 Período Oficial</strong>Abertura em 11 de junho de 2026.</li>
                    <li><strong>🏠 Regulamento Regulamentar</strong>12 grupos com 4 seleções. Avançam os dois melhores de cada chave mais os 8 melhores terceiros colocados gerais.</li>
                    <li><strong>🇧🇷 Grupo do Brasil</strong>Alocado no Grupo C competindo diretamente com Marrocos, Haiti e Escócia.</li>
                </ul>
            </div>
        </div>

        <div id="tab-jogos" class="tab-content">
            <div class="filter-wrapper">
                <input type="text" id="searchMobileTeam" placeholder="Buscar seleção (ex: Brasil)..." onkeyup="filterMobileGames()">
                <select id="filterMobileRodada" onchange="filterMobileGames()">
                    <option value="">Todas as rodadas</option>
                    <option value="1ª">1ª Rodada</option>
                    <option value="2ª">2ª Rodada</option>
                    <option value="3ª">3ª Rodada</option>
                </select>
            </div>
            <div id="mobileGamesContainer"></div>
        </div>

        <div id="tab-grupos" class="tab-content">
            <div class="filter-wrapper">
                <select id="groupMobileSelect" onchange="renderMobileGroupView()">
                    <option value="A">Grupo A</option>
                    <option value="B">Grupo B</option>
                    <option value="C" selected>Grupo C (Brasil)</option>
                    <option value="D">Grupo D</option>
                    <option value="E">Grupo E</option>
                    <option value="F">Grupo F</option>
                    <option value="G">Grupo G</option>
                    <option value="H">Grupo H</option>
                    <option value="I">Grupo I</option>
                    <option value="J">Grupo J</option>
                    <option value="K">Grupo K</option>
                    <option value="L">Grupo L</option>
                </select>
            </div>

            <div class="mobile-group-box">
                <div class="mobile-group-header" id="mobileGroupTitle">Seleções</div>
                <div class="mobile-group-body" id="mobileGroupTeamsContainer"></div>
            </div>

            <div class="mobile-group-box">
                <div class="mobile-group-header">Jogos Chaveados</div>
                <div class="mobile-group-body" id="mobileGroupGamesContainer" style="padding: 8px;"></div>
            </div>
            <button class="save-action-btn" onclick="saveMobileGroupScores()">💾 Salvar Palpites do Grupo</button>
        </div>
    </div>

    <div id="mobileModalOverlay" class="modal-overlay" onclick="closeMobileModal(event)">
        <div class="modal">
            <div class="modal-header">
                <h3 id="mobileModalTitle">Detalhes</h3>
                <button class="modal-close-btn" onclick="closeMobileModal(true)">&times;</button>
            </div>
            <div id="mobileModalBody"></div>
        </div>
    </div>

    <div id="mobileToast" class="toast"></div>

    <footer>
        Horários oficiais em Brasília (BRT) | Foco Mobile © 2026
    </footer>

    <script>
        // Mapeamento Completo de ISOs para FlagCDN
        const flagsRegistry = {
            "México": "mx", "África do Sul": "za", "Coreia do Sul": "kr", "Rep. Tcheca/Irlanda": "cz",
            "Canadá": "ca", "Itália/País de Gales": "gb-wls", "EUA": "us", "Paraguai": "py",
            "Austrália": "au", "Turquia/Romênia": "tr", "Catar": "qa", "Suíça": "ch",
            "Brasil": "br", "Marrocos": "ma", "Haiti": "ht", "Escócia": "gb-sct",
            "Alemanha": "de", "Curaçau": "cw", "Holanda": "nl", "Japão": "jp",
            "Costa do Marfim": "ci", "Equador": "ec", "Ucrânia/Polônia": "pl", "Tunísia": "tn",
            "Espanha": "es", "Cabo Verde": "cv", "Bélgica": "be", "Egito": "eg",
            "Arábia Saudita": "sa", "Uruguai": "uy", "Irã": "ir", "Nova Zelândia": "nz",
            "Argentina": "ar", "Argélia": "dz", "França": "fr", "Senegal": "sn",
            "Iraque/Bolívia": "bo", "Noruega": "no", "Áustria": "at", "Jordânia": "jo",
            "Portugal": "pt", "RD Congo/Jamaica": "jm", "Inglaterra": "gb-eng", "Croácia": "hr",
            "Gana": "gh", "Panamá": "pa", "Uzbequistão": "uz", "Colômbia": "co"
        };

        const gruposData = {
            "A": [
                { name: "México", desc: "País Sede - Força Local" }, { name: "África do Sul", desc: "Representante Africano" },
                { name: "Coreia do Sul", desc: "Velocidade Asiática" }, { name: "Rep. Tcheca/Irlanda", desc: "Qualificatória Europeia" }
            ],
            "B": [
                { name: "Canadá", desc: "País Sede - Evolução" }, { name: "Itália/País de Gales", desc: "Tradição em Campo" },
                { name: "EUA", desc: "País Sede - Gigante Técnico" }, { name: "Paraguai", desc: "Garra Conmebol" }
            ],
            "C": [
                { name: "Brasil", desc: "Favorito do Grupo — 5 Títulos", destaque: true }, { name: "Marrocos", desc: "Histórico em 2022" },
                { name: "Haiti", desc: "Classificado Concacaf" }, { name: "Escócia", desc: "Consistência Tática" }
            ],
            "D": [
                { name: "Austrália", desc: "Representante Oceania/Ásia" }, { name: "Turquia/Romênia", desc: "Força Física Competitiva" },
                { name: "Catar", desc: "Atual Campeão Asiático" }, { name: "Suíça", desc: "Foco Defensivo Estruturado" }
            ],
            "E": [
                { name: "Alemanha", desc: "Tradicional Tetracampeã" }, { name: "Curaçau", desc: "Surpresa das Eliminatórias" },
                { name: "Holanda", desc: "Futebol Dinâmico" }, { name: "Japão", desc: "Disciplina e Técnica" }
            ],
            "F": [
                { name: "Costa do Marfim", desc: "Físico e Velocidade" }, { name: "Equador", desc: "Altitude e Juventude" },
                { name: "Ucrânia/Polônia", desc: "Repescagem de Elite" }, { name: "Tunísia", desc: "Estratégia Coletiva" }
            ],
            "G": [
                { name: "Espanha", desc: "Posse de Bola e Domínio" }, { name: "Cabo Verde", desc: "Histórico Crescimento" },
                { name: "Bélgica", desc: "Fase de Renovação Técnica" }, { name: "Egito", desc: "Destaque do Norte Africano" }
            ],
            "H": [
                { name: "Arábia Saudita", desc: "Evolução Estrutural" }, { name: "Uruguai", desc: "Tradição e Entrega" },
                { name: "Irã", desc: "Poder de Marcação" }, { name: "Nova Zelândia", desc: "Força Aérea Destacada" }
            ],
            "I": [
                { name: "Argentina", desc: "Atual Defensora do Título" }, { name: "Argélia", desc: "Habilidade Técnica" },
                { name: "França", desc: "Elenco de Alto Nível" }, { name: "Senegal", desc: "Potência Física Continental" }
            ],
            "J": [
                { name: "Iraque/Bolívia", desc: "Competitividade Intensa" }, { name: "Noruega", desc: "Ataque Jovem e Fatal" },
                { name: "Áustria", desc: "Organização Germânica" }, { name: "Jordânia", desc: "Estreante Motivado" }
            ],
            "K": [
                { name: "Portugal", desc: "Talento e Ambição Europeia" }, { name: "RD Congo/Jamaica", desc: "Velocidade e Transições" },
                { name: "Inglaterra", desc: "Poder da Liga Local" }, { name: "Croácia", desc: "Controle de Meio-Campo" }
            ],
            "L": [
                { name: "Gana", desc: "Tradição em Copas" }, { name: "Panamá", desc: "Velocidade e Explosão" },
                { name: "Uzbequistão", desc: "Nova Força Asiática" }, { name: "Colômbia", desc: "Habilidade e Técnica Pura" }
            ]
        };

        const jogosData = [
            // 1ª Rodada
            {data: "11/06", grupo: "A", hora: "16:00", time1: "México", time2: "África do Sul", local: "Cidade do México", rodada: "1ª"},
            {data: "11/06", grupo: "A", hora: "23:00", time1: "Coreia do Sul", time2: "Rep. Tcheca/Irlanda", local: "Guadalajara", rodada: "1ª"},
            {data: "12/06", grupo: "B", hora: "16:00", time1: "Canadá", time2: "Itália/País de Gales", local: "Toronto", rodada: "1ª"},
            {data: "12/06", grupo: "B", hora: "22:00", time1: "EUA", time2: "Paraguai", local: "Los Angeles", rodada: "1ª"},
            {data: "13/06", grupo: "D", hora: "01:00", time1: "Austrália", time2: "Turquia/Romênia", local: "Vancouver", rodada: "1ª"},
            {data: "13/06", grupo: "D", hora: "16:00", time1: "Catar", time2: "Suíça", local: "San Francisco", rodada: "1ª"},
            {data: "13/06", grupo: "C", hora: "19:00", time1: "Brasil", time2: "Marrocos", local: "Nova York/NJ", rodada: "1ª", brasil: true},
            {data: "13/06", grupo: "C", hora: "22:00", time1: "Haiti", time2: "Escócia", local: "Boston", rodada: "1ª"},
            {data: "14/06", grupo: "E", hora: "14:00", time1: "Alemanha", time2: "Curaçau", local: "Houston", rodada: "1ª"},
            {data: "14/06", grupo: "E", hora: "17:00", time1: "Holanda", time2: "Japão", local: "Dallas", rodada: "1ª"},
            {data: "14/06", grupo: "F", hora: "20:00", time1: "Costa do Marfim", time2: "Equador", local: "Filadélfia", rodada: "1ª"},
            {data: "14/06", grupo: "F", hora: "23:00", time1: "Ucrânia/Polônia", time2: "Tunísia", local: "Monterrey", rodada: "1ª"},
            {data: "15/06", grupo: "G", hora: "13:00", time1: "Espanha", time2: "Cabo Verde", local: "Atlanta", rodada: "1ª"},
            {data: "15/06", grupo: "G", hora: "16:00", time1: "Bélgica", time2: "Egito", local: "Seattle", rodada: "1ª"},
            {data: "15/06", grupo: "H", hora: "19:00", time1: "Arábia Saudita", time2: "Uruguai", local: "Miami", rodada: "1ª"},
            {data: "15/06", grupo: "H", hora: "22:00", time1: "Irã", time2: "Nova Zelândia", local: "Los Angeles", rodada: "1ª"},
            {data: "16/06", grupo: "I", hora: "14:00", time1: "Argentina", time2: "Argélia", local: "Kansas City", rodada: "1ª"},
            {data: "16/06", grupo: "I", hora: "16:00", time1: "França", time2: "Senegal", local: "Nova York/NJ", rodada: "1ª"},
            {data: "16/06", grupo: "J", hora: "19:00", time1: "Iraque/Bolívia", time2: "Noruega", local: "Boston", rodada: "1ª"},
            {data: "17/06", grupo: "J", hora: "01:00", time1: "Áustria", time2: "Jordânia", local: "San Francisco", rodada: "1ª"},
            {data: "17/06", grupo: "K", hora: "14:00", time1: "Portugal", time2: "RD Congo/Jamaica", local: "Houston", rodada: "1ª"},
            {data: "17/06", grupo: "K", hora: "17:00", time1: "Inglaterra", time2: "Croácia", local: "Dallas", rodada: "1ª"},
            {data: "17/06", grupo: "L", hora: "20:00", time1: "Gana", time2: "Panamá", local: "Toronto", rodada: "1ª"},
            {data: "17/06", grupo: "L", hora: "23:00", time1: "Uzbequistão", time2: "Colômbia", local: "Cidade do México", rodada: "1ª"},

            // 2ª Rodada
            {data: "18/06", grupo: "A", hora: "13:00", time1: "Rep. Tcheca/Irlanda", time2: "África do Sul", local: "Atlanta", rodada: "2ª"},
            {data: "18/06", grupo: "D", hora: "16:00", time1: "Suíça", time2: "Itália/País de Gales", local: "Los Angeles", rodada: "2ª"},
            {data: "18/06", grupo: "D", hora: "19:00", time1: "Canadá", time2: "Catar", local: "Vancouver", rodada: "2ª"},
            {data: "18/06", grupo: "A", hora: "22:00", time1: "México", time2: "Coreia do Sul", local: "Guadalajara", rodada: "2ª"},
            {data: "19/06", grupo: "B", hora: "01:00", time1: "Turquia/Romênia", time2: "Paraguai", local: "San Francisco", rodada: "2ª"},
            {data: "19/06", grupo: "B", hora: "16:00", time1: "EUA", time2: "Austrália", local: "Seattle", rodada: "2ª"},
            {data: "19/06", grupo: "C", hora: "19:00", time1: "Escócia", time2: "Marrocos", local: "Boston", rodada: "2ª"},
            {data: "19/06", grupo: "C", hora: "22:00", time1: "Brasil", time2: "Haiti", local: "Filadélfia", rodada: "2ª", brasil: true},
            {data: "20/06", grupo: "E", hora: "14:00", time1: "Holanda", time2: "Ucrânia/Polônia", local: "Houston", rodada: "2ª"},
            {data: "20/06", grupo: "E", hora: "17:00", time1: "Alemanha", time2: "Costa do Marfim", local: "Toronto", rodada: "2ª"},
            {data: "20/06", grupo: "F", hora: "21:00", time1: "Equador", time2: "Curaçau", local: "Kansas City", rodada: "2ª"},
            {data: "21/06", grupo: "F", hora: "01:00", time1: "Tunísia", time2: "Japão", local: "Monterrey", rodada: "2ª"},
            {data: "21/06", grupo: "G", hora: "13:00", time1: "Espanha", time2: "Arábia Saudita", local: "Atlanta", rodada: "2ª"},
            {data: "21/06", grupo: "G", hora: "16:00", time1: "Bélgica", time2: "Irã", local: "Los Angeles", rodada: "2ª"},
            {data: "21/06", grupo: "H", hora: "19:00", time1: "Uruguai", time2: "Cabo Verde", local: "Miami", rodada: "2ª"},
            {data: "21/06", grupo: "H", hora: "22:00", time1: "Nova Zelândia", time2: "Egito", local: "Vancouver", rodada: "2ª"},
            {data: "22/06", grupo: "I", hora: "14:00", time1: "Argentina", time2: "Áustria", local: "Dallas", rodada: "2ª"},
            {data: "22/06", grupo: "I", hora: "18:00", time1: "França", time2: "Iraque/Bolívia", local: "Filadélfia", rodada: "2ª"},
            {data: "22/06", grupo: "J", hora: "21:00", time1: "Noruega", time2: "Senegal", local: "Nova York/NJ", rodada: "2ª"},
            {data: "23/06", grupo: "J", hora: "00:00", time1: "Jordânia", time2: "Argélia", local: "San Francisco", rodada: "2ª"},
            {data: "23/06", grupo: "K", hora: "14:00", time1: "Portugal", time2: "Uzbequistão", local: "Houston", rodada: "2ª"},
            {data: "23/06", grupo: "K", hora: "17:00", time1: "Inglaterra", time2: "Gana", local: "Boston", rodada: "2ª"},
            {data: "23/06", grupo: "L", hora: "20:00", time1: "Panamá", time2: "Croácia", local: "Toronto", rodada: "2ª"},
            {data: "23/06", grupo: "L", hora: "23:00", time1: "Colômbia", time2: "RD Congo/Jamaica", local: "Guadalajara", rodada: "2ª"},

            // 3ª Rodada
            {data: "24/06", grupo: "D", hora: "16:00", time1: "Suíça", time2: "Canadá", local: "Vancouver", rodada: "3ª"},
            {data: "24/06", grupo: "D", hora: "16:00", time1: "Itália/País de Gales", time2: "Catar", local: "Seattle", rodada: "3ª"},
            {data: "24/06", grupo: "C", hora: "19:00", time1: "Escócia", time2: "Brasil", local: "Miami", rodada: "3ª", brasil: true},
            {data: "24/06", grupo: "C", hora: "19:00", time1: "Marrocos", time2: "Haiti", local: "Atlanta", rodada: "3ª"},
            {data: "24/06", grupo: "A", hora: "22:00", time1: "Rep. Tcheca/Irlanda", time2: "México", local: "Cidade do México", rodada: "3ª"},
            {data: "24/06", grupo: "A", hora: "22:00", time1: "África do Sul", time2: "Coreia do Sul", local: "Monterrey", rodada: "3ª"},
            {data: "25/06", grupo: "E", hora: "17:00", time1: "Equador", time2: "Alemanha", local: "Nova York/NJ", rodada: "3ª"},
            {data: "25/06", grupo: "E", hora: "17:00", time1: "Curaçau", time2: "Costa do Marfim", local: "Filadélfia", rodada: "3ª"},
            {data: "25/06", grupo: "F", hora: "20:00", time1: "Japão", time2: "Ucrânia/Polônia", local: "Dallas", rodada: "3ª"},
            {data: "25/06", grupo: "F", hora: "20:00", time1: "Tunísia", time2: "Holanda", local: "Kansas City", rodada: "3ª"},
            {data: "25/06", grupo: "B", hora: "23:00", time1: "Turquia/Romênia", time2: "EUA", local: "Los Angeles", rodada: "3ª"},
            {data: "25/06", grupo: "B", hora: "23:00", time1: "Paraguai", time2: "Austrália", local: "San Francisco", rodada: "3ª"},
            {data: "26/06", grupo: "I", hora: "16:00", time1: "Noruega", time2: "França", local: "Boston", rodada: "3ª"},
            {data: "26/06", grupo: "I", hora: "16:00", time1: "Senegal", time2: "Iraque/Bolívia", local: "Toronto", rodada: "3ª"},
            {data: "26/06", grupo: "G", hora: "21:00", time1: "Cabo Verde", time2: "Arábia Saudita", local: "Houston", rodada: "3ª"},
            {data: "26/06", grupo: "G", hora: "21:00", time1: "Uruguai", time2: "Espanha", local: "Guadalajara", rodada: "3ª"},
            {data: "27/06", grupo: "H", hora: "00:00", time1: "Egito", time2: "Irã", local: "Seattle", rodada: "3ª"},
            {data: "27/06", grupo: "H", hora: "00:00", time1: "Nova Zelândia", time2: "Bélgica", local: "Vancouver", rodada: "3ª"},
            {data: "27/06", grupo: "K", hora: "18:00", time1: "Panamá", time2: "Inglaterra", local: "Nova York/NJ", rodada: "3ª"},
            {data: "27/06", grupo: "K", hora: "18:00", time1: "Croácia", time2: "Gana", local: "Filadélfia", rodada: "3ª"},
            {data: "27/06", grupo: "L", hora: "20:30", time1: "Colômbia", time2: "Portugal", local: "Miami", rodada: "3ª"},
            {data: "27/06", grupo: "L", hora: "20:30", time1: "RD Congo/Jamaica", time2: "Uzbequistão", local: "Atlanta", rodada: "3ª"},
            {data: "27/06", grupo: "J", hora: "23:00", time1: "Argélia", time2: "Áustria", local: "Kansas City", rodada: "3ª"},
            {data: "27/06", grupo: "J", hora: "23:00", time1: "Jordânia", time2: "Argentina", local: "Dallas", rodada: "3ª"}
        ];

        function getFlagImgHTML(teamName) {
            const iso = flagsRegistry[teamName] || 'un';
            return `<img src="https://flagcdn.com/w40/${iso}.png" class="flag-icon" alt="${teamName}">`;
        }

        /* ========== GERENCIAMENTO DE ABAS ========== */
        function switchMobileTab(event, tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-button').forEach(el => el.classList.remove('active'));
            
            document.getElementById(tabId).classList.add('active');
            
            if (event) {
                event.target.classList.add('active');
            } else {
                // Caso a mudança venha de cliques nos cartões internos
                document.querySelectorAll('.tab-button').forEach(btn => {
                    if (btn.textContent.includes('Tabela') && tabId === 'tab-jogos') btn.classList.add('active');
                });
            }

            if (tabId === 'tab-jogos') renderMobileGamesList();
            if (tabId === 'tab-grupos') renderMobileGroupView();
        }

        /* ========== ABA 2: CONSTRUÇÃO DOS CARD DE JOGOS (ESTILO MOBILE) ========== */
        function renderMobileGamesList() {
            const container = document.getElementById('mobileGamesContainer');
            container.innerHTML = '';

            const localData = JSON.parse(localStorage.getItem('copaResultsMobile') || '{}');

            jogosData.forEach((jogo, index) => {
                const card = document.createElement('div');
                card.className = `game-card ${jogo.brasil ? 'brasil-match' : ''}`;
                card.dataset.teamSearch = `${jogo.time1} ${jogo.time2}`.toLowerCase();
                card.dataset.rodadaNum = jogo.rodada;

                const score1 = localData[`game_${index}_1`] || '';
                const score2 = localData[`game_${index}_2`] || '';

                card.innerHTML = `
                    <div class="game-card-meta">
                        <span>Group ${jogo.grupo} — ${jogo.rodada} Rodada</span>
                        <span>${jogo.data} às ${jogo.hora}</span>
                    </div>
                    <div class="game-card-row">
                        <div class="game-card-team">
                            ${getFlagImgHTML(jogo.time1)}
                            <span>${jogo.time1}</span>
                        </div>
                        <div class="game-card-score">
                            <input type="number" class="mobile-score-input" value="${score1}" data-game-idx="${index}" data-score-pos="1" oninput="saveMobileGameInput(this)">
                            <span style="color:#a0aec0;font-weight:bold;">:</span>
                            <input type="number" class="mobile-score-input" value="${score2}" data-game-idx="${index}" data-score-pos="2" oninput="saveMobileGameInput(this)">
                        </div>
                        <div class="game-card-team right">
                            <span>${jogo.time2}</span>
                            ${getFlagImgHTML(jogo.time2)}
                        </div>
                    </div>
                    <div class="game-card-venue">🏟️ ${jogo.local}</div>
                `;
                container.appendChild(card);
            });
        }

        function filterMobileGames() {
            const search = document.getElementById('searchMobileTeam').value.toLowerCase();
            const rodada = document.getElementById('filterMobileRodada').value;
            const cards = document.querySelectorAll('#mobileGamesContainer .game-card');

            cards.forEach(card => {
                const matchesSearch = !search || card.dataset.teamSearch.includes(search);
                const matchesRodada = !rodada || card.dataset.rodadaNum === rodada;
                card.style.display = (matchesSearch && matchesRodada) ? 'block' : 'none';
            });
        }

        function saveMobileGameInput(input) {
            const idx = input.dataset.gameIdx;
            const pos = input.dataset.scorePos;
            const value = input.value;

            const localData = JSON.parse(localStorage.getItem('copaResultsMobile') || '{}');
            localData[`game_${idx}_${pos}`] = value;
            localStorage.setItem('copaResultsMobile', JSON.stringify(localData));
        }

        /* ========== ABA 3: CRIAÇÃO DO CHAVEAMENTO DE GRUPOS DINÂMICOS ========== */
        function renderMobileGroupView() {
            const currentGroup = document.getElementById('groupMobileSelect').value;
            document.getElementById('mobileGroupTitle').textContent = `Seleções do Grupo ${currentGroup}`;

            // 1. Renderiza times do grupo
            const teamsContainer = document.getElementById('mobileGroupTeamsContainer');
            teamsContainer.innerHTML = '';
            
            const times = gruposData[currentGroup] || [];
            times.forEach(team => {
                const item = document.createElement('div');
                item.className = `mobile-team-item ${team.destaque ? 'destaque' : ''}`;
                item.innerHTML = `
                    <div class="mobile-team-left">
                        <img src="https://flagcdn.com/w40/${flagsRegistry[team.name] || 'un'}.png" class="flag-icon-large" alt="${team.name}">
                        <div>
                            <div class="mobile-team-name">${team.name}</div>
                            <div class="mobile-team-desc">${team.desc}</div>
                        </div>
                    </div>
                `;
                teamsContainer.appendChild(item);
            });

            // 2. Renderiza jogos do grupo específico
            const gamesContainer = document.getElementById('mobileGroupGamesContainer');
            gamesContainer.innerHTML = '';

            const localData = JSON.parse(localStorage.getItem('copaResultsMobile') || '{}');

            jogosData.forEach((jogo, index) => {
                if (jogo.grupo !== currentGroup) return;

                const score1 = localData[`game_${index}_1`] || '';
                const score2 = localData[`game_${index}_2`] || '';

                const row = document.createElement('div');
                row.style = "display:flex; justify-content:space-between; align-items:center; padding:8px 4px; border-bottom:1px solid #edf2f7;";
                row.innerHTML = `
                    <div style="font-size:0.7em; color:#718096; width:55px;"><strong>${jogo.data}</strong><br>${jogo.hora}</div>
                    <div style="flex:1; font-size:0.8em; font-weight:600; display:flex; flex-direction:column; gap:4px;">
                        <div style="display:flex; align-items:center; gap:4px;">${getFlagImgHTML(jogo.time1)} ${jogo.time1}</div>
                        <div style="display:flex; align-items:center; gap:4px;">${getFlagImgHTML(jogo.time2)} ${jogo.time2}</div>
                    </div>
                    <div style="display:flex; flex-direction:column; gap:4px;">
                        <input type="number" class="mobile-score-input group-sync-input" value="${score1}" data-global-id="${index}" data-pos="1" style="height:26px; width:34px; font-size:0.8em;">
                        <input type="number" class="mobile-score-input group-sync-input" value="${score2}" data-global-id="${index}" data-pos="2" style="height:26px; width:34px; font-size:0.8em;">
                    </div>
                `;
                gamesContainer.appendChild(row);
            });
        }

        function saveMobileGroupScores() {
            const localData = JSON.parse(localStorage.getItem('copaResultsMobile') || '{}');
            
            document.querySelectorAll('.group-sync-input').forEach(input => {
                const idx = input.dataset.globalId;
                const pos = input.dataset.pos;
                if (input.value !== '') {
                    localData[`game_${idx}_${pos}`] = input.value;
                }
            });

            localStorage.setItem('copaResultsMobile', JSON.stringify(localData));
            showMobileToast('Palpites salvos com sucesso! ⚽');
        }

        /* ========== MODAIS INTERATIVOS (ESTILO SHEET DO IOS) ========== */
        function triggerMobileModal(type) {
            const titleEl = document.getElementById('mobileModalTitle');
            const bodyEl = document.getElementById('mobileModalBody');
            let content = '';

            if (type === 'selecoes') {
                titleEl.textContent = 'Seleções Competindo (48)';
                content = '<div class="modal-grid-list">';
                Object.keys(flagsRegistry).sort().forEach(name => {
                    content += `
                        <div class="modal-list-item">
                            <img src="https://flagcdn.com/w40/${flagsRegistry[name]}.png" class="flag-icon" alt="${name}">
                            <span>${name}</span>
                        </div>`;
                });
                content += '</div>';
            } 
            else if (type === 'sedes') {
                titleEl.textContent = 'Cidades-Sede e Capacidades';
                content = `
                    <div class="modal-block-item">
                        <strong>🇺🇸 ESTADOS UNIDOS</strong>
                        <p style="margin-top:4px; font-size:0.9em; color:#4a5568;">
                            • N. York/N. Jersey: MetLife (82.500)<br>• Dallas: AT&T Stadium (80.000)<br>
                            • Kansas City: Arrowhead (76.400)<br>• Houston: NRG Stadium (72.200)<br>
                            • Atlanta: Mercedes-Benz (71.000)<br>• L. Angeles: SoFi Stadium (70.200)<br>
                            • Filadélfia: Lincoln Field (69.100)<br>• Seattle: Lumen Field (69.000)<br>
                            • S. Francisco: Levi's Stadium (68.500)<br>• Boston: Gillette Stadium (65.800)<br>
                            • Miami: Hard Rock Stadium (64.700)
                        </p>
                    </div>
                    <div class="modal-block-item" style="border-left-color:#06A77D;">
                        <strong>🇲🇽 MÉXICO</strong>
                        <p style="margin-top:4px; font-size:0.9em; color:#4a5568;">
                            • Cidade do México: Estádio Azteca (87.523)<br>
                            • Monterrey: Estádio BBVA (53.500)<br>
                            • Guadalajara: Estádio Akron (48.071)
                        </p>
                    </div>
                    <div class="modal-block-item" style="border-left-color:#F77F00;">
                        <strong>🇨🇦 CANADÁ</strong>
                        <p style="margin-top:4px; font-size:0.9em; color:#4a5568;">
                            • Vancouver: BC Place (54.500)<br>
                            • Toronto: BMO Field (30.000)
                        </p>
                    </div>
                `;
            } 
            else if (type === 'paises') {
                titleEl.textContent = 'Países Organizadores';
                content = `
                    <div class="modal-block-item">
                        <strong>🌎 Organização Conjunta Tripla</strong>
                        <p style="margin-top:5px; font-size:0.95em; color:#4a5568; line-height:1.4;">
                            Pela primeira vez na história da FIFA, três países organizam o torneio de forma integrada. Os <strong>Estados Unidos</strong> concentrarão a maior parte dos jogos das fases decisivas. O <strong>México</strong> faz história ao ter o Estádio Azteca como palco pela terceira vez, e o <strong>Canadá</strong> sedia o torneio masculino de forma inédita.
                        </p>
                    </div>
                `;
            }

            bodyEl.innerHTML = content;
            document.getElementById('mobileModalOverlay').style.display = 'flex';
        }

        function closeMobileModal(e) {
            if (e === true || e.target === document.getElementById('mobileModalOverlay')) {
                document.getElementById('mobileModalOverlay').style.display = 'none';
            }
        }

        function showMobileToast(msg) {
            const toast = document.getElementById('mobileToast');
            toast.textContent = msg;
            toast.style.opacity = '1';
            setTimeout(() => { toast.style.opacity = '0'; }, 2000);
        }

        // Execução Inicial
        renderMobileGamesList();
    </script>
</body>
</html>
