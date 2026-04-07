# signos.html
Programação web
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Oráculo Zodiacal</title>
    <!-- Bootstrap via CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        /* Estilização Própria - O toque de autoridade */
        body {
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            color: #fff;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Segoe UI', sans-serif;
        }
        .card-custom {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 15px;
            padding: 2rem;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            max-width: 450px;
            width: 100%;
        }
        .btn-astro {
            background: linear-gradient(45deg, #ff00cc, #333399);
            border: none;
            color: white;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: transform 0.2s;
        }
        .btn-astro:hover {
            transform: scale(1.05);
            background: linear-gradient(45deg, #ff33dd, #4444aa);
        }
        .resultado-box {
            display: none; /* Oculto inicialmente */
            animation: fadeIn 0.8s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .signo-titulo {
            font-size: 2.5rem;
            color: #ff00cc;
            text-shadow: 0 0 10px rgba(255, 0, 204, 0.5);
        }
    </style>
</head>
<body>

    <div class="container d-flex justify-content-center">
        <div class="card-custom text-center">
            
            <!-- Formulário (Objetivo 1) -->
            <div id="formulario-area">
                <h2 class="mb-4">Descubra Seu Destino</h2>
                <p class="text-muted mb-4">As estrelas não mentem. Insira sua data de nascimento.</p>
                <form id="form-signo">
                    <div class="mb-3">
                        <label for="data-nasc" class="form-label text-start d-block">Data de Nascimento</label>
                        <input type="date" id="data-nasc" class="form-control form-control-lg bg-dark text-white border-secondary" required>
                    </div>
                    <button type="submit" class="btn btn-astro w-100 py-2">Consultar Oráculo</button>
                </form>
            </div>

            <!-- Resultado (Objetivo 2 e 4) -->
            <div id="resultado-area" class="resultado-box">
                <h3 class="text-uppercase text-muted" style="font-size: 0.9rem;">Seu signo é</h3>
                <h1 class="signo-titulo mb-3" id="res-nome">-</h1>
                <div class="row text-start mb-3">
                    <div class="col-6">
                        <small class="text-muted">Elemento</small>
                        <p class="fw-bold" id="res-elemento">-</p>
                    </div>
                    <div class="col-6">
                        <small class="text-muted">Regente</small>
                        <p class="fw-bold" id="res-regente">-</p>
                    </div>
                </div>
                <div class="alert alert-dark border-secondary">
                    <p id="res-desc" class="mb-0 fst-italic">-</p>
                </div>
                <button onclick="location.reload()" class="btn btn-outline-light btn-sm mt-3">Nova Consulta</button>
            </div>

        </div>
    </div>

    <script>
        // Lógica de Consulta (Objetivo 3)
        // Simulando o XML externo para funcionar sem servidor local (CORS).
        // Em produção, você usaria fetch('signos.xml')
        const xmlData = `<?xml version="1.0" encoding="UTF-8"?>
        <signos>
            <signo nome="Áries" inicio="03-21" fim="04-19" elemento="Fogo" regente="Marte" descricao="Impulsivo, líder, guerreiro. Não pede licença, toma espaço." />
            <signo nome="Touro" inicio="04-20" fim="05-20" elemento="Terra" regente="Vênus" descricao="Teimoso, leal, amante do luxo. Constrói impérios com paciência." />
            <signo nome="Gêmeos" inicio="05-21" fim="06-20" elemento="Ar" regente="Mercúrio" descricao="Dual, comunicador, adaptável. Está em dois lugares ao mesmo tempo." />
            <signo nome="Câncer" inicio="06-21" fim="07-22" elemento="Água" regente="Lua" descricao="Protetor, emocional, intuitivo. A casca é dura, o interior é mar." />
            <signo nome="Leão" inicio="07-23" fim="08-22" elemento="Fogo" regente="Sol" descricao="Rei, dramático, generoso. Nasceu para o palco, mesmo que o palco seja vazio." />
            <signo nome="Virgem" inicio="08-23" fim="09-22" elemento="Terra" regente="Mercúrio" descricao="Analítico, perfeccionista, útil. Enxerga o erro antes de você cometê-lo." />
            <signo nome="Libra" inicio="09-23" fim="10-22" elemento="Ar" regente="Vênus" descricao="Diplomata, indeciso, charmoso. equilibra a balança enquanto todos desabam." />
            <signo nome="Escorpião" inicio="10-23" fim="11-21" elemento="Água" regente="Plutão" descricao="Intenso, misterioso, transformador. Veja tudo, não diga nada." />
            <signo nome="Sagitário" inicio="11-22" fim="12-21" elemento="Fogo" regente="Júpiter" descricao="Otimista, viajante, franco. A verdade dói, mas liberta." />
            <signo nome="Capricórnio" inicio="12-22" fim="01-19" elemento="Terra" regente="Saturno" descricao="Ambicioso, disciplined, realista. O tempo é seu aliado, não seu inimigo." />
            <signo nome="Aquário" inicio="01-20" fim="02-18" elemento="Ar" regente="Urano" descricao="Rebelde, inovador, humanitário. O futuro chegou, e você está atrasado." />
            <signo nome="Peixes" inicio="02-19" fim="03-20" elemento="Água" regente="Netuno" descricao="Sonhador, empático, iludido. Vive em um mundo que ninguém mais enxerga." />
        </signos>`;

        document.getElementById('form-signo').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const dataInput = document.getElementById('data-nasc').value;
            if (!dataInput) return;

            const data = new Date(dataInput);
            const dia = data.getUTCDate();
            const mes = data.getUTCMonth() + 1; // JS conta meses de 0 a 11
            
            const signo = calcularSigno(dia, mes);
            exibirResultado(signo);
        });

        function calcularSigno(dia, mes) {
            // Parser do XML
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlData, "text/xml");
            const signos = xmlDoc.getElementsByTagName("signo");
            
            let resultado = null;

            // Lógica de comparação de datas
            for (let i = 0; i < signos.length; i++) {
                const s = signos[i];
                const inicioParts = s.getAttribute("inicio").split("-");
                const fimParts = s.getAttribute("fim").split("-");
                
                const inicioDia = parseInt(inicioParts[1]);
                const inicioMes = parseInt(inicioParts[0]);
                const fimDia = parseInt(fimParts[1]);
                const fimMes = parseInt(fimParts[0]);

                // Tratamento para Capricórnio (cruz o ano)
                if (inicioMes > fimMes) {
                    if ((mes === inicioMes && dia >= inicioDia) || (mes === fimMes && dia <= fimDia)) {
                        resultado = s;
                        break;
                    }
                } else {
                    if ((mes === inicioMes && dia >= inicioDia) || (mes === fimMes && dia <= fimDia) || (mes > inicioMes && mes < fimMes)) {
                         // Verificação mais robusta para intervalos normais
                         const dataInicio = new Date(2000, inicioMes - 1, inicioDia);
                         const dataFim = new Date(2000, fimMes - 1, fimDia);
                         const dataAlvo = new Date(2000, mes - 1, dia);
                         
                         if (dataAlvo >= dataInicio && dataAlvo <= dataFim) {
                             resultado = s;
                             break;
                         }
                    }
                }
            }
            return resultado;
        }

        function exibirResultado(signoObj) {
            if (!signoObj) {
                alert("Data inválida ou erro cósmico.");
                return;
            }

            // Troca de tela (simulando redirecionamento para página de resultado)
            document.getElementById('formulario-area').style.display = 'none';
            const resArea = document.getElementById('resultado-area');
            resArea.style.display = 'block';

            // Preenchendo dados (Objetivo 2)
            document.getElementById('res-nome').innerText = signoObj.getAttribute("nome");
            document.getElementById('res-elemento').innerText = signoObj.getAttribute("elemento");
            document.getElementById('res-regente').innerText = signoObj.getAttribute("regente");
            document.getElementById('res-desc').innerText = signoObj.getAttribute("descricao");
        }
    </script>
</body>
</html>
