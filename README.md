# Projeto1---Bloco-animado-infantil
Missao 2 - projeto para o github
# Bloco Animado Infantil — Cores Primárias e Formas Geométricas

Projeto em **React** com foco em **interação para crianças de até 3 anos**, apresentando 4 formas geométricas (círculo, quadrado, triângulo e estrela) em **cores primárias** com animação suave.

## 🎯 Objetivo
Criar uma aplicação simples e visualmente atrativa para crianças, aplicando conceitos fundamentais de **front-end com React**.

## 🚀 Tecnologias Utilizadas
- **React**
- **JavaScript (ES6+)**
- **HTML5**
- **CSS3**
- **Framer Motion** (animações)

## 📚 O que aprendi
- **Gerenciamento de Estado**: utilização do hook `useState` para controlar forma, cor e exibição do nome.
- **Componentização**: criação de componentes reutilizáveis como `BigButton` e `Shape`.
- **Props**: passagem de dados e funções entre componentes (pai para filho).
- **Renderização Condicional**: mostrar/ocultar nome da forma.
- **Animação**: aplicação de animações suaves com `framer-motion`.
- **Acessibilidade**: uso de ARIA labels, alto contraste e elementos grandes.

## 📂 Estrutura do Projeto
```
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bloco Infantil Interativo</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Balsamiq+Sans:wght@700&display=swap" rel="stylesheet">
    <style>
        /* Estilos Gerais */
        :root {
            --cor-fundo: #f0f9ff;
            --cor-vermelho: #e74c3c;
            --cor-azul: #3498db;
            --cor-amarelo: #f1c40f;
            --cor-sombra: rgba(0, 0, 0, 0.15);
            --cor-texto: #34495e;
            --fonte-principal: 'Balsamiq Sans', cursive;
        }

        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
            overflow: hidden; /* Evita rolagem em tablets */
        }

        body {
            font-family: var(--fonte-principal);
            background-color: var(--cor-fundo);
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--cor-texto);
            -webkit-user-select: none; /* Impede seleção de texto */
            -ms-user-select: none;
            user-select: none;
        }

        /* Container Principal */
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
            max-width: 900px;
            padding: 20px;
            box-sizing: border-box;
        }

        /* Área de Exibição da Forma */
        .area-forma {
            width: 35vmin;
            height: 35vmin;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 5vh;
        }

        #forma {
            width: 100%;
            height: 100%;
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55); /* Efeito elástico */
            box-shadow: 0 10px 25px var(--cor-sombra);
        }

        /* Classes para as Cores */
        .cor-vermelho { background-color: var(--cor-vermelho); }
        .cor-azul { background-color: var(--cor-azul); }
        .cor-amarelo { background-color: var(--cor-amarelo); }

        /* Classes para as Formas Geométricas */
        .forma-quadrado { border-radius: 12%; }
        .forma-circulo { border-radius: 50%; }
        .forma-triangulo {
            background-color: transparent;
            width: 0;
            height: 0;
            border-left: 17.5vmin solid transparent;
            border-right: 17.5vmin solid transparent;
            border-bottom: 35vmin solid;
            box-shadow: none;
            transition: border-color 0.4s ease;
        }
        .forma-triangulo.cor-vermelho { border-bottom-color: var(--cor-vermelho); }
        .forma-triangulo.cor-azul { border-bottom-color: var(--cor-azul); }
        .forma-triangulo.cor-amarelo { border-bottom-color: var(--cor-amarelo); }

        .forma-estrela {
            clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
        }


        /* Painel de Controles */
        .controles {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4vh;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 3vh 4vw;
            border-radius: 20px;
            box-shadow: 0 5px 15px var(--cor-sombra);
        }

        .grupo-botoes {
            display: flex;
            gap: 20px;
        }

        .botao {
            width: 15vmin;
            height: 15vmin;
            max-width: 100px;
            max-height: 100px;
            border: 5px solid white;
            border-radius: 20px;
            cursor: pointer;
            box-shadow: 0 4px 10px var(--cor-sombra);
            transition: all 0.2s ease-in-out;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .botao svg {
            width: 60%;
            height: 60%;
            fill: white;
        }

        .botao:active {
            transform: scale(0.9);
        }

        .botao.selecionado {
            border-color: var(--cor-texto);
            transform: scale(1.1);
        }

    </style>
</head>
<body>

    <div class="container">
        <!-- A forma principal será exibida aqui -->
        <div class="area-forma">
            <div id="forma"></div>
        </div>

        <!-- Os botões para controlar a forma e a cor -->
        <div class="controles">
            <!-- Botões de Formas -->
            <div class="grupo-botoes">
                <button class="botao" data-tipo="forma" data-valor="quadrado" aria-label="Selecionar Quadrado">
                    <svg viewBox="0 0 100 100"><rect x="10" y="10" width="80" height="80" rx="10"/></svg>
                </button>
                <button class="botao" data-tipo="forma" data-valor="circulo" aria-label="Selecionar Círculo">
                    <svg viewBox="0 0 100 100"><circle cx="50" cy="50" r="45"/></svg>
                </button>
                <button class="botao" data-tipo="forma" data-valor="triangulo" aria-label="Selecionar Triângulo">
                    <svg viewBox="0 0 100 100"><polygon points="50,10 95,90 5,90"/></svg>
                </button>
                <button class="botao" data-tipo="forma" data-valor="estrela" aria-label="Selecionar Estrela">
                    <svg viewBox="0 0 100 100"><polygon points="50,5 61,40 98,40 68,62 79,96 50,75 21,96 32,62 2,40 39,40"/></svg>
                </button>
            </div>
            <!-- Botões de Cores -->
            <div class="grupo-botoes">
                <button class="botao cor-vermelho" data-tipo="cor" data-valor="vermelho" aria-label="Selecionar Cor Vermelha"></button>
                <button class="botao cor-azul" data-tipo="cor" data-valor="azul" aria-label="Selecionar Cor Azul"></button>
                <button class="botao cor-amarelo" data-tipo="cor" data-valor="amarelo" aria-label="Selecionar Cor Amarela"></button>
            </div>
        </div>
    </div>

    <script>
        // --- LÓGICA DO JAVASCRIPT ---

        // Seleciona os elementos importantes da página
        const elementoForma = document.getElementById('forma');
        const botoesForma = document.querySelectorAll('.botao[data-tipo="forma"]');
        const botoesCor = document.querySelectorAll('.botao[data-tipo="cor"]');

        // Guarda o estado atual da forma e da cor
        let formaAtual = 'quadrado';
        let corAtual = 'amarelo';

        // Função principal para atualizar a aparência da forma na tela
        function atualizarVisual() {
            // Limpa as classes antigas do elemento principal
            elementoForma.className = '';
            
            // Adiciona as novas classes de forma e cor
            // Ex: 'forma-quadrado cor-amarelo'
            elementoForma.classList.add(`forma-${formaAtual}`);
            elementoForma.classList.add(`cor-${corAtual}`);

            // Atualiza o botão de forma selecionado
            botoesForma.forEach(botao => {
                if (botao.dataset.valor === formaAtual) {
                    botao.classList.add('selecionado');
                } else {
                    botao.classList.remove('selecionado');
                }
            });

            // Atualiza o botão de cor selecionado
            botoesCor.forEach(botao => {
                if (botao.dataset.valor === corAtual) {
                    botao.classList.add('selecionado');
                } else {
                    botao.classList.remove('selecionado');
                }
            });
        }

        // Adiciona um "ouvinte" de clique para cada botão de forma
        botoesForma.forEach(botao => {
            botao.addEventListener('click', () => {
                // Pega o valor da forma do botão clicado (ex: 'circulo')
                formaAtual = botao.dataset.valor;
                // Chama a função para redesenhar a tela
                atualizarVisual();
            });
        });

        // Adiciona um "ouvinte" de clique para cada botão de cor
        botoesCor.forEach(botao => {
            botao.addEventListener('click', () => {
                // Pega o valor da cor do botão clicado (ex: 'azul')
                corAtual = botao.dataset.valor;
                // Chama a função para redesenhar a tela
                atualizarVisual();
            });
        });

        // Inicia o aplicativo com a primeira forma e cor
        window.onload = () => {
            atualizarVisual();
        };

    </script>
</body>
</html>

```

## 🖼 Funcionalidades
- Selecionar entre 4 formas geométricas.
- Escolher cor primária.
- Mostrar ou ocultar nome da forma.
- Animação de "quicar" no bloco.

## 🔧 Como Executar Localmente
```bash
# Clonar o repositório
git clone https://github.com/usuario/bloco-animado-infantil.git

# Entrar no diretório
cd bloco-animado-infantil

# Instalar dependências
npm install

# Rodar a aplicação
npm run dev
```

## 📜 Licença
Este projeto é de uso livre para fins educacionais.

---
**Autor:** Thaiz Sousa Amorim
