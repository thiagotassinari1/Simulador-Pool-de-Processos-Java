# 🚀 Simulação de Pool de Processos

![Status](https://img.shields.io/badge/status-em%20andamento-blue)

Este projeto é uma simulação de um sistema operacional simples para gerenciamento e execução de um pool de processos. Foi desenvolvido como o Trabalho Prático do Grau B para a disciplina de Programação Orientada a Objetos da Escola Politécnica da **Universidade do Vale do Rio dos Sinos (Unisinos)**.

O sistema permite ao usuário enfileirar diferentes tipos de processos, que são executados em ordem (FIFO) ou por uma chamada específica via PID.

---

## ⚙️ Arquitetura e Conceitos

O projeto é fundamentado no conceito de **Polimorfismo** em Java.

* Uma classe abstrata `Processo` define o "contrato" básico, possuindo um `pid` e um método abstrato `execute()`.
* Quatro classes concretas estendem `Processo` e implementam (`@Override`) sua própria versão do método `execute()`.

A "fila de processos" principal do sistema é uma **fila dinâmica** (implementada como `ArrayList`).

---

## 🔧 Tipos de Processos (Subclasses)

Existem 4 tipos de processos que o usuário pode criar:

1.  **`ComputingProcess` (Processo de Cálculo)**
    * **O que faz:** Executa o cálculo de uma expressão aritmética simples (ex: "10 + 5") que é formada por dois operandos e uma operação (+, -, \*, /).
    * **Observação:** A expressão é recebida como uma String em seu construtor.

2.  **`WritingProcess` (Processo de Gravação)**
    * **O que faz:** Salva uma expressão de cálculo em um arquivo chamado `computation.txt`.
    * **Observação:** O processo **anexa** a nova expressão ao final do arquivo, sem sobrescrever o conteúdo existente.

3.  **`ReadingProcess` (Processo de Leitura)**
    * **O que faz:** Lê todas as linhas (expressões) do arquivo `computation.txt`.
    * Para cada linha lida, ele cria um novo objeto `ComputingProcess` e o adiciona na fila principal de processos.
    * Após terminar a leitura, o processo **limpa** o arquivo `computation.txt`.
    * **Observação:** Esta classe recebe uma referência para a fila de processos principal em seu construtor para poder interagir com ela.

4.  **`PrintingProcess` (Processo de Impressão)**
    * **O que faz:** Imprime no console o estado atual de todo o pool de processos.
    * A impressão mostra o `pid`, o `tipo` do processo e seus `atributos` relacionados.
    * **Observação:** Esta classe também recebe uma referência para a fila de processos principal em seu construtor.

---

## 📋 Funcionalidades do Menu

O sistema principal (`Main`) opera através de um menu com as seguintes opções:

1.  **Criar processo:**
    * Permite ao usuário escolher um dos 4 tipos de processo para criar.
    * Solicita os dados necessários (como a expressão para um `ComputingProcess`) e adiciona o novo processo ao **final** da fila.

2.  **Executar próximo:**
    * Pega o processo na **posição 0** da fila (o mais antigo).
    * Chama o método `execute()` polimórfico daquele processo.
    * Remove o processo da fila após a execução.

3.  **Executar processo específico:**
    * Solicita ao usuário um `pid`.
    * Busca na fila o processo com aquele `pid`.
    * Se encontrar, chama o `execute()` daquele processo e o remove da fila, mesmo que ele não seja o primeiro.

4.  **Salvar a fila de processos:**
    * Salva o estado atual da fila de processos em um arquivo.

5.  **Carregar fila de processos:**
    * Inicializa o sistema carregando uma fila salva anteriormente em um arquivo.
