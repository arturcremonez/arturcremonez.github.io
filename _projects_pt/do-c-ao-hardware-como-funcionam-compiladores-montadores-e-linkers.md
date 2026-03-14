---
layout: post
title: "Do C ao Hardware: Como funcionam Compiladores, Montadores e Linkers"
author: "Artur Cremonez"
description: "Entenda o fluxo de transformação do código C até a execução no hardware, detalhando o papel do compilador, montador, linker e a arquitetura MIPS."
category: arquitetura-computadores
order: 1
lang: pt
redirect_from:
  - /arquitetura-de-computadores/sistemas/2026/02/26/do-c-ao-hardware-como-funcionam-compiladores-montadores-e-linkers.html
---

## Como montadores e linkers surgem?

Para ilustrar, vamos usar um exemplo comum: o **gcc**.

Suponha que temos os arquivos de um projeto:

- `main.c`
- `list.h`
- `list.c`

### 1. Compilando `main.c`

```bash
gcc -c main.c -o main.o
```

- O compilador transforma `main.c` em `main.s` (assembly).  
- O **montador** transforma `main.s` em `main.o` (arquivo objeto).

### 2. Compilando `list.c`

```bash
gcc -c list.c -o list.o
```

- O compilador transforma `list.c` em `list.s`.  
- O montador transforma `list.s` em `list.o`.

### 3. Gerando o executável

```bash
gcc main.o list.o -o programa
```

- O **linker** transforma os arquivos objeto em um executável chamado `programa`.

> Conclusão: O `gcc` não é apenas um compilador; ele funciona como **compilador + montador + linker**.

---

## Visão Geral: Como montadores e linkers se relacionam com o todo

| Etapa         | Função                                                                 | Arquivo de entrada | Arquivo de saída |
|---------------|------------------------------------------------------------------------|------------------|----------------|
| Compilador    | Traduz de C para assembly                                              | `*.c`            | `*.s`          |
| Montador      | Traduz de assembly para arquivo objeto                                  | `*.s`            | `*.o`          |
| Linker        | Junta arquivos objeto em executável                                     | `*.o`            | Executável     |
| Loader        | Coloca o executável na memória (parte do kernel do SO)                 | Executável       | Memória        |

> Observação: a linguagem C é portável, mas o assembly e os arquivos objeto dependem da arquitetura do computador.

---

## Entendendo o Montador

O montador traduz **assembly para linguagem de máquina**.  

A linguagem que a máquina entende é binária (1s e 0s).  
Nosso objetivo é mapear instruções assembly para código binário de forma precisa, respeitando a **arquitetura do processador**.

Usaremos a arquitetura **MIPS** como exemplo.

### Tipo R

| Campo  | Bits | Função |
|--------|------|--------|
| opcode | 6    | Tipo da operação (ex: ADD = 000000) |
| rs     | 5    | Registrador fonte 1 ($t1) |
| rt     | 5    | Registrador fonte 2 ($t2) |
| rd     | 5    | Registrador destino ($t0) |
| shamt  | 5    | Deslocamento (0 para ADD) |
| funct  | 6    | Função exata (diz à ULA qual operação fazer) |

> Somando: 6 + 5 + 5 + 5 + 5 + 6 = 32 bits = 4 bytes. Cada linha de comando se torna uma instrução.

### Tipo I

| Campo     | Bits | Função |
|-----------|------|--------|
| opcode    | 6    | Função exata |
| rs        | 5    | Registrador fonte 1 ($t1) |
| rt        | 5    | Registrador fonte 2 ($t2) |
| immediate | 16   | Operando ou offset |

> Somando: 6 + 5 + 5 + 16 = 32 bits = 4 bytes

### Tipo J

| Campo  | Bits | Função |
|--------|------|--------|
| opcode | 6    | Função exata |
| adress | 26   | Endereço ou rótulo |

> Somando: 6 + 26 = 32 bits = 4 bytes

> Observações:
> - `adress` quando o rótulo é local, o montador substitui pelo endereço.  
> - Quando não é local, apenas armazena o rótulo; futuramente o linker define o endereço final.  
> - Para formar o endereço final: shift left 2 e concatenação com os 4 bits mais altos do PC corrente.  
> - Cada endereço de memória RAM é visto como 4 bytes, portanto `$pc` incrementa 4 a cada instrução.

### Fluxo de execução (MIPS)

- `IR` (Instruction Register) armazena a instrução.  
- `PC` (Program Counter) armazena o endereço da próxima instrução.  
- `opcode` vai direto para a **unidade de controle**, que direciona os sinais no circuito.  
- `rs`, `rt` e `rd` são selecionados dentro do **IR** e enviados ao **banco de registradores**.  
- `shamt` vai para a entrada de deslocamento da **ULA** (shifter).  
- `funct` vai para o decodificador da **ULA**, indicando a operação exata: ADD, SUB, AND, OR, SLT, etc.  
- `immediate` vai direto para a **ULA** em instruções tipo I.  
- `$pc` é incrementado em 4 a cada instrução.

---

## Entendendo o Linker

O linker converte arquivos objeto em executáveis e permite a união de diferentes programas (Ex: C e Fortran).

Existem dois tipos:

### Linkedição Estática

Cria um executável monolítico, incluindo todo o código e bibliotecas necessárias.

**Vantagens:**
- **Portabilidade:** executável não depende de bibliotecas externas.  
- **Performance:** todos os dados já estão carregados na memória.  
- **Controle:** garante versões consistentes das bibliotecas.

**Desvantagens:**
- Aumento do tamanho do arquivo e consumo de memória.  
- Tempos de compilação mais longos, especialmente em projetos grandes.

### Linkedição Dinâmica

Não copia o código da biblioteca; insere referência à biblioteca externa.

**Vantagens:**
- **Arquivos menores** e uso de memória reduzido.  
- **Facilidade de atualização:** basta instalar a nova versão da biblioteca.  
- **Eficiência compartilhada:** múltiplos programas podem usar a mesma biblioteca carregada.

**Desvantagens:**
- Sobrecarga de desempenho na execução.  
- Problemas de versionamento: múltiplos programas podem requerer versões diferentes.  
- Problemas de portabilidade: dependências externas devem ser fornecidas.

> Observação: linguagens modernas como Go e Rust usam linkagem estática por padrão.

## Conclusão

Compreender a função do **compilador, montador, linker e loader** ajuda a enxergar **o caminho do código de alto nível até a execução na máquina**, e como cada etapa depende da arquitetura do computador.  

Esse fluxo é essencial para quem deseja aprofundar-se em **sistemas, arquitetura e otimização de código**.
