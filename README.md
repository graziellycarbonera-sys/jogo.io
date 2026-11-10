# jogo.io

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Desafio do Conhecimento</title>

    <!-- Conecta o arquivo CSS -->
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <main class="container">

        <h1>🚀 Desafio do Conhecimento</h1>
        <p class="subtitulo">Teste seus conhecimentos!</p>

        <!-- Tela inicial -->
        <section id="telaInicio">
            <h2>Bem-vindo ao desafio!</h2>
            <p>Responda 5 perguntas e tente acertar todas!</p>
            <p>Você pode errar até 2 vezes.</p>

            <button id="botaoIniciar">Começar jogo</button>
        </section>

        <!-- Tela do jogo -->
        <section id="telaJogo" class="escondido">

            <div class="informacoes">
                <span id="pontuacao">⭐ Pontos: 0</span>
                <span id="vidas">❤️ Erros: 0/3</span>
            </div>

            <p id="progresso">Pergunta 1 de 5</p>

            <h2 id="pergunta">Pergunta aqui</h2>

            <div id="alternativas"></div>

            <p id="mensagem"></p>

            <button id="botaoProxima" class="escondido">
                Próxima pergunta
            </button>

        </section>

        <!-- Tela final -->
        <section id="telaFinal" class="escondido">
            <h2 id="resultado"></h2>
            <p id="resultadoPontos"></p>

            <button id="botaoReiniciar">
                Jogar novamente
            </button>
        </section>

    </main>

    <!-- Conecta o arquivo JavaScript -->
    <script src="script.js"></script>

/* Configuração geral da página */
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, sans-serif;

    background: linear-gradient(135deg, #160b35, #29206b);

    display: flex;
    justify-content: center;
    align-items: center;

    padding: 20px;
    color: white;
}

/* Caixa principal do jogo */
.container {
    background-color: #21194b;
    width: 100%;
    max-width: 550px;

    padding: 30px;
    border-radius: 20px;

    text-align: center;

    box-shadow: 0 0 25px #08051c;
}

/* Título */
h1 {
    color: #ffe66d;
    font-size: 28px;
}

.subtitulo {
    color: #bdb5ed;
}

/* Informações de pontos e erros */
.informacoes {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    margin: 20px 0;
    color: #ffe66d;
    font-weight: bold;
}

/* Pergunta */
#pergunta {
    margin: 25px 0;
    font-size: 22px;
}

/* Botões */
button {
    background-color: #6750e8;
    color: white;

    border: none;
    border-radius: 10px;

    padding: 14px 20px;
    margin: 8px;

    font-size: 16px;
    font-weight: bold;

    cursor: pointer;
    transition: 0.2s;
}

/* Efeito ao passar o mouse */
button:hover {
    background-color: #8978ff;
    transform: scale(1.03);
}

/* Alternativas */
#alternativas {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

#alternativas button {
    width: 100%;
    margin: 0;
    background-color: #352b70;
    border: 2px solid #7465ca;
}

#alternativas button:hover:not(:disabled) {
    background-color: #5142a0;
}

/* Respostas desativadas */
button:disabled {
    cursor: not-allowed;
    opacity: 0.7;
}

/* Mensagens */
#mensagem {
    font-weight: bold;
    min-height: 25px;
    margin-top: 20px;
}

.escondido {
    display: none !important;
}

/* Ajustes para celular */
@media (max-width: 480px) {
    .container {
        padding: 20px;
    }

    h1 {
        font-size: 23px;
    }

    #pergunta {
        font-size: 19px;
    }

    .informacoes {
        font-size: 14px;
    }
}

// Lista de perguntas do jogo
const perguntas = [
    {
        pergunta: "Qual é o maior planeta do Sistema Solar?",
        alternativas: ["Terra", "Marte", "Júpiter", "Vênus"],
        correta: 2
    },
    {
        pergunta: "Quanto é 8 x 7?",
        alternativas: ["54", "56", "64", "48"],
        correta: 1
    },
    {
        pergunta: "Qual linguagem é usada para estruturar páginas web?",
        alternativas: ["HTML", "Python", "Java", "C++"],
        correta: 0
    },
    {
        pergunta: "Qual animal é conhecido como rei da selva?",
        alternativas: ["Elefante", "Tigre", "Leão", "Lobo"],
        correta: 2
    },
    {
        pergunta: "Quantos lados tem um hexágono?",
        alternativas: ["Cinco", "Seis", "Sete", "Oito"],
        correta: 1
    }
];

// Variáveis que controlam o jogo
let perguntaAtual = 0;
let pontos = 0;
let erros = 0;
let respondido = false;

// Pegando os elementos do HTML
const telaInicio = document.getElementById("telaInicio");
const telaJogo = document.getElementById("telaJogo");
const telaFinal = document.getElementById("telaFinal");

const botaoIniciar = document.getElementById("botaoIniciar");
const botaoProxima = document.getElementById("botaoProxima");
const botaoReiniciar = document.getElementById("botaoReiniciar");

const pergunta = document.getElementById("pergunta");
const alternativas = document.getElementById("alternativas");

const pontuacao = document.getElementById("pontuacao");
const vidas = document.getElementById("vidas");
const progresso = document.getElementById("progresso");

const mensagem = document.getElementById("mensagem");
const resultado = document.getElementById("resultado");
const resultadoPontos = document.getElementById("resultadoPontos");

// Começa o jogo
botaoIniciar.addEventListener("click", iniciarJogo);

// Reinicia o jogo
botaoReiniciar.addEventListener("click", iniciarJogo);

// Vai para a próxima pergunta
botaoProxima.addEventListener("click", proximaPergunta);

// Função para iniciar ou reiniciar
function iniciarJogo() {
    perguntaAtual = 0;
    pontos = 0;
    erros = 0;

    telaInicio.classList.add("escondido");
    telaFinal.classList.add("escondido");
    telaJogo.classList.remove("escondido");

    atualizarInformacoes();
    mostrarPergunta();
}

// Mostra a pergunta atual
function mostrarPergunta() {
    respondido = false;

    const atual = perguntas[perguntaAtual];

    pergunta.textContent = atual.pergunta;

    progresso.textContent =
        `Pergunta ${perguntaAtual + 1} de ${perguntas.length}`;

    alternativas.innerHTML = "";

    mensagem.textContent = "";
    botaoProxima.classList.add("escondido");

    // Cria um botão para cada alternativa
    atual.alternativas.forEach((opcao, indice) => {

        const botao = document.createElement("button");

        botao.textContent = opcao;

        botao.addEventListener("click", function() {
            verificarResposta(indice);
        });

        alternativas.appendChild(botao);
    });
}

// Verifica se a resposta está correta
function verificarResposta(indice) {
    if (respondido) return;

    respondido = true;

    const respostaCorreta = perguntas[perguntaAtual].correta;
    const botoes = alternativas.querySelectorAll("button");

    // Impede que o jogador escolha mais de uma resposta
    botoes.forEach(botao => {
        botao.disabled = true;
    });

    if (indice === respostaCorreta) {
        pontos += 10;
        mensagem.textContent = "🎉 Muito bem! Resposta correta!";
        mensagem.style.color = "#6cff9b";
    } else {
        erros++;
        mensagem.textContent =
            "❌ Resposta errada! A correta era: " +
            perguntas[perguntaAtual].alternativas[respostaCorreta];

        mensagem.style.color = "#ff8585";
    }

    atualizarInformacoes();

    // Verifica se o jogador perdeu
    if (erros >= 3) {
        finalizarJogo(false);
        return;
    }

    // Se terminou todas as perguntas
    if (perguntaAtual === perguntas.length - 1) {
        finalizarJogo(true);
        return;
    }

    botaoProxima.classList.remove("escondido");
}

// Atualiza pontos e erros na tela
function atualizarInformacoes() {
    pontuacao.textContent = `⭐ Pontos: ${pontos}`;
    vidas.textContent = `❤️ Erros: ${erros}/3`;
}

// Avança para a próxima pergunta
function proximaPergunta() {
    perguntaAtual++;
    mostrarPergunta();
}

// Mostra a vitória ou derrota
function finalizarJogo(venceu) {
    telaJogo.classList.add("escondido");
    telaFinal.classList.remove("escondido");

    if (venceu) {
        resultado.textContent = "🏆 Parabéns! Você venceu!";
    } else {
        resultado.textContent = "😢 Fim de jogo! Você perdeu!";
    }

    resultadoPontos.textContent =
        `Sua pontuação final foi: ${pontos} pontos!`;
}

</body>
</html>
