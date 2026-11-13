<h1 align="center">📚 Biblioteca Cesar</h1>
<p align="center"><b>Sub-rotinas, passagem de parâmetros e uso de bibliotecas no computador Cesar</b></p>

<p align="center">
  <img alt="Arquitetura" src="https://img.shields.io/badge/arquitetura-16%20bits-1F3A5F?style=for-the-badge">
  <img alt="Biblioteca" src="https://img.shields.io/badge/biblioteca-BibCesar%20V8-2A7F8E?style=for-the-badge">
  <img alt="Montador" src="https://img.shields.io/badge/montador-Daedalus-6B7280?style=for-the-badge">
  <img alt="Exercícios" src="https://img.shields.io/badge/exerc%C3%ADcios-8-success?style=for-the-badge">
</p>

<p align="center">
  <b>Gabriel Cabral Wallendzu</b> · HT3080251<br>
  Sistemas de Computação 1 de Automação · Prof. Carlos Eduardo Pagani<br>
  IFSP – Câmpus Hortolândia · 2026
</p>

---

## 📑 Sumário

[Sobre](#-sobre-o-projeto) · [BibCesar](#-a-bibcesar) · [Programas](#-programas) · [Convenções](#-convenções-adotadas) · [Como executar](#%EF%B8%8F-como-executar) · [Testes](#-testes-realizados) · [Tarefas](#-divisão-das-tarefas) · [Estrutura](#-estrutura-do-repositório)

---

## 🎯 Sobre o projeto

Resolução do laboratório **"Biblioteca Cesar"**: uso de uma biblioteca de sub-rotinas (BibCesar), passagem de parâmetros por **registradores** e pela **pilha**, e implementação de sub-rotinas próprias de conversão numérica, exibição em hexadecimal, cópia de strings e leitura de teclado, tudo no simulador **Cesar** com o montador **Daedalus**.

> 📄 O relatório completo (questões teóricas, explicações, prints e testes) acompanha este repositório.

---

## 🧰 A BibCesar

Arquivo-esqueleto `BibCesarV8__ex.ced`: inicializa a pilha (`MOV #65498, R6`), reserva uma área para o código do usuário e traz as sub-rotinas da biblioteca. Todas são chamadas com **`JSR R7, nome`** e retornam com `RTS R7`.

| Sub-rotina | Entrada | Saída |
|---|---|---|
| `_S_LIMPA_VISOR_RAPIDA` | — | Visor limpo (36 `CLR` seguidos) |
| `_S_LIMPA_VISOR_ITERATIVA` | — | Visor limpo com laço |
| `_S_ESCREVER_NO_VISOR` | `R0` = tamanho · `R1` = mensagem (`DAB`) · `R2` = LED inicial | Texto no visor |
| `_S_MULTIPLICAR` | `R0` × `R1` | `R0:R1` = produto de 32 bits · `V` = 1 se `R0` ≠ 0 |
| `_S_DIVIDIR` | `R0:R1` ÷ `R2` | `R0` = quociente · `R1` = resto · `Z` = divisão por zero · `V` = estouro |

---

## 📦 Programas

Cada pasta traz o código-fonte (`.ced`) e os arquivos gerados na montagem (`.mem` e `.map`).

### 🧮 Exemplos resolvidos

| Pasta | O que faz |
|---|---|
| [`Exemplo 1`](Exemplo%201) | `Soma(p1, p2)` com parâmetros em **registradores** (`R0` e `R1`), resultado em `R0` |
| [`Exemplo 2`](Exemplo%202) | O mesmo, com parâmetros passados pela **pilha** |

### ✏️ Exercícios propostos

| Pasta | Sub-rotina | O que faz |
|---|---|---|
| [`Ex1`](Ex1) | `uint2str(char *s, uint n)` | Inteiro sem sinal → string, por divisões sucessivas por 10 |
| [`Ex2`](Ex2) | `int2str(char *s, int n)` | Inteiro com sinal → string (trata o `-` e reaproveita o `uint2str`) |
| [`Ex3`](Ex3) | `str2int(int *n, char *s)` | String → inteiro com sinal (acumulado × 10 + algarismo) |
| [`Ex4`](Ex4) | `Display(WORD pos, WORD valor)` | Mostra o valor em **hexadecimal** no visor, a partir do LED `pos` |
| [`Ex5`](Ex5) | `char *copia(char *dst, char *src)` | Copia a string e devolve `dst` em `R0` |
| [`Ex6`](Ex6) | `char getch()` | Espera uma tecla e devolve o código ASCII em `R0` |

---

## 📐 Convenções adotadas

| Tema | Convenção |
|---|---|
| **Strings** | Um caractere por palavra de 16 bits (byte menos significativo), terminada em `0` |
| **Parâmetros na pilha** | Empilhados da direita para a esquerda; quem chama remove com `ADD #2n, R6` |
| **Registradores** | Cada sub-rotina salva e restaura o que usa; o retorno fica em `R0` |
| **Dados** | No final do arquivo, a partir de `ORG 1024` |

Estado da pilha ao entrar em uma sub-rotina com dois parâmetros:

```
[R6]      endereço de retorno
[R6 + 2]  1º parâmetro (o último empilhado)
[R6 + 4]  2º parâmetro (o primeiro empilhado)
```

---

## ▶️ Como executar

1. 🛠️ Abra o **Daedalus** e escolha **Máquina Alvo = Cesar**.
2. 📂 Abra o arquivo `.ced` da pasta desejada.
3. 🔨 Clique em **montar** (gera `.mem` e `.map`).
4. 🖥️ Abra o simulador **Cesar**, carregue o programa e execute.
5. ✅ Confira o visor, os registradores e a memória com a tabela de testes abaixo.

> **Big endian:** uma palavra de 16 bits ocupa dois bytes, e o simulador mostra byte a byte. Um valor pequeno aparece no byte **seguinte** ao endereço da palavra (por exemplo, 350 na palavra 1028 aparece como `1` e `94`).

---

## ✅ Testes realizados

| Programa | Entradas | Saídas verificadas | Status |
|---|---|---|---|
| Exemplo 1 | 100 e 250 (registradores) | `R0` = 350 · `1028` = 350 | ✅ |
| Exemplo 2 | 100 e 250 (pilha) | `R0` = 350 · `1028` = 350 · `SP` = 65498 | ✅ |
| Exercício 1 | 54321 · 0 · 65535 | Visor: `54321`, `0`, `65535` | ✅ |
| Exercício 2 | −12345 · 777 · −32768 | Visor: `-12345`, `777`, `-32768` | ✅ |
| Exercício 3 | "-12345" · "4096" · "0" | −12345, 4096 e 0; visor: `-12345`, `4096` | ✅ |
| Exercício 4 | 7ABCh · 1234h · 00FFh | Visor: `7ABC`, `1234`, `00FF` | ✅ |
| Exercício 5 | "Cesar 2026" | Visor: `Cesar 2026` · `R0` = endereço do destino | ✅ |
| Exercício 6 | Tecla `7` três vezes | Visor: `777` · `R0` = 55 | ✅ |


---

## 📋 Divisão das tarefas

Trabalho **individual**: todas as tarefas foram realizadas por Gabriel Cabral Wallendzu.

| 📝 A fazer | 🚧 Em andamento | ✅ Concluído |
|---|---|---|
| Trabalho avaliativo: calculadora no visor | — | Questões teóricas |
| — | — | Exemplos 1 e 2 |
| — | — | Exercícios 1 a 6 |
| — | — | Repositório e relatório |

---

## 🗂️ Estrutura do repositório

```
.
├── Exemplo 1/      código, .mem e .map
├── Exemplo 2/
├── Ex1/  ...  Ex6/
├── Relatorio_BibCesar
└── README.md
```

| Extensão | O que é |
|---|---|
| `.ced` | Código-fonte do Daedalus para o Cesar |
| `.mem` | Imagem da memória gerada na montagem |
| `.map` | Mapa de rótulos e endereços |

---

<p align="center"><sub>Gabriel Cabral Wallendzu · HT3080251 · IFSP Hortolândia · 2026</sub></p>
