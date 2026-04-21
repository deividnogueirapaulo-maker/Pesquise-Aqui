<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pesquise aqui! - 1ª Cia Operacional</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-red: #d32f2f;
            --bg-white: #ffffff;
            --text-dark: #333333;
            --shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--bg-white);
            margin: 0;
            padding: 20px;
            color: var(--text-dark);
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        /* Cabeçalho com Logo à Esquerda e Textos Padronizados */
        header {
            display: grid;
            grid-template-columns: 1fr auto 1fr; /* Mantém o centro real equilibrado */
            align-items: center;
            width: 100%;
            max-width: 1000px;
            margin-bottom: 30px;
            padding-bottom: 15px;
            border-bottom: 2px solid #f0f0f0;
        }

        .header-logo {
            grid-column: 1;
            justify-self: start;
        }

        .header-logo img {
            height: 90px;
            width: auto;
            display: block;
        }

        .header-text {
            grid-column: 2;
            text-align: center;
        }

        /* Padronização das frases do cabeçalho */
        header h1, header .sub-header {
            font-weight: 700;
            color: var(--primary-red);
            text-transform: uppercase;
            margin: 0;
            letter-spacing: 1px;
        }

        header h1 {
            font-size: 1.8rem;
        }

        header .sub-header {
            font-size: 1.6rem;
            margin-top: 10px; /* Espaçamento de uma linha */
        }

        .header-spacer {
            grid-column: 3;
        }

        /* Barra de Escala */
        .info-bar {
            width: 100%;
            max-width: 1000px;
            text-align: left;
            font-size: 14pt;
            margin-bottom: 25px;
            border-left: 6px solid var(--primary-red);
            padding-left: 15px;
            color: #555;
        }

        /* Estilo Base para todos os Botões (Aviso e Link) */
        .btn-base {
            background-color: #fff;
            border: 2px solid var(--primary-red);
            border-radius: 10px;
            text-decoration: none;
            box-shadow: var(--shadow);
            transition: all 0.3s ease;
            display: block;
        }

        /* Animação unificada para Hover */
        .btn-base:hover {
            background-color: var(--primary-red);
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(211, 47, 47, 0.2);
        }

        /* Ajustes específicos para Aviso */
        #avisos-wrapper {
            width: 100%;
            max-width: 1000px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 30px;
        }

        .btn-aviso {
            padding: 12px 0;
            overflow: hidden;
        }

        .marquee {
            display: inline-block;
            white-space: nowrap;
            padding-left: 100%;
            animation: marquee 25s linear infinite;
            color: var(--primary-red);
            font-weight: 700;
            font-size: 1.1rem;
            transition: color 0.3s ease;
        }

        .btn-aviso:hover .marquee {
            color: #fff; /* Texto fica branco no hover */
        }

        @keyframes marquee {
            0% { transform: translate(0, 0); }
            100% { transform: translate(-100%, 0); }
        }

        /* Grid de Links */
        .grid-links {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
            gap: 15px;
            width: 100%;
            max-width: 1000px;
        }

        .btn-link {
            color: var(--primary-red);
            padding: 25px 15px;
            text-align: center;
            font-weight: 700;
            font-size: 1rem;
            text-transform: uppercase;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .btn-link:hover {
            color: #fff;
        }

        .btn-link:active, .btn-aviso:active {
            transform: scale(0.96);
        }

        /* Responsividade */
        @media (max-width: 768px) {
            header { display: flex; flex-direction: column; gap: 15px; }
            .header-logo img { height: 70px; }
            .grid-links { grid-template-columns: 1fr 1fr; }
        }
    </style>
</head>
<body>

    <header>
        <div class="header-logo" id="logo-container"></div>
        
        <div class="header-text">
            <h1>Pesquise aqui</h1>
            <div class="sub-header">1ª Cia Operacional</div>
        </div>

        <div class="header-spacer"></div>
    </header>

    <div class="info-bar" id="data-pelotao">Sincronizando escala...</div>

    <div id="avisos-wrapper"></div>

    <div class="grid-links" id="links-container"></div>

    <script>
        const SHEET_ID = '1qh_Xkuu0RIKmcnBxuXpuBKIOq91sUi_zpHCKsDD2jcA';

        function formatarLinkImagem(url) {
            if (url.includes('drive.google.com')) {
                const id = url.split('/d/')[1]?.split('/')[0] || url.split('id=')[1];
                return `https://lh3.googleusercontent.com/u/0/d/${id}`;
            }
            return url;
        }

        function atualizarEscala() {
            const agora = new Date();
            const operacional = new Date(agora.getTime());
            if (agora.getHours() < 8) operacional.setDate(operacional.getDate() - 1);

            const dataRef = new Date('2026-04-21T08:00:00');
            const diffDias = Math.floor((operacional - dataRef) / (1000 * 60 * 60 * 24));
            
            let pelotao = ((diffDias + 2) % 4); 
            if (pelotao < 0) pelotao += 4;
            pelotao += 1;

            const dataTexto = operacional.toLocaleDateString('pt-BR', { year: 'numeric', month: 'long', day: 'numeric' });
            document.getElementById('data-pelotao').innerText = `Data: ${dataTexto}, - ${pelotao}º Pelotão Operacional`;
        }

        async function fetchSheetData(tabName) {
            const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:csv&sheet=${encodeURIComponent(tabName)}`;
            const response = await fetch(url);
            const text = await response.text();
            return text.split('\n').map(row => row.split(',').map(cell => cell.replace(/"/g, '')));
        }

        async function carregarPagina() {
            try {
                // 1. Logo
                const logosData = await fetchSheetData('logos');
                if (logosData[1] && logosData[1][1]) {
                    const imgUrl = formatarLinkImagem(logosData[1][1].trim());
                    document.getElementById('logo-container').innerHTML = `<img src="${imgUrl}" alt="Logo">`;
                }

                // 2. Avisos com Animação Unificada
                const avisoData = await fetchSheetData('aviso');
                const wrapper = document.getElementById('avisos-wrapper');
                avisoData.slice(1).forEach(row => {
                    const msg = row[0];
                    if (msg && msg.trim() !== "") {
                        const a = document.createElement('a');
                        a.href = row[1] || "#";
                        a.target = "_blank";
                        a.className = "btn-base btn-aviso";
                        a.innerHTML = `<div class="marquee">⚠️ AVISO: ${msg} &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ⚠️ AVISO: ${msg}</div>`;
                        wrapper.appendChild(a);
                    }
                });

                // 3. Links
                const linksData = await fetchSheetData('links');
                const linksContainer = document.getElementById('links-container');
                linksData.slice(1).forEach(row => {
                    if(row[0] && row[1]) {
                        const a = document.createElement('a');
                        a.href = row[1];
                        a.className = "btn-base btn-link";
                        a.innerText = row[0];
                        a.target = "_blank";
                        linksContainer.appendChild(a);
                    }
                });
            } catch (e) { console.error("Erro:", e); }
        }

        atualizarEscala();
        carregarPagina();
    </script>
</body>
</html>
