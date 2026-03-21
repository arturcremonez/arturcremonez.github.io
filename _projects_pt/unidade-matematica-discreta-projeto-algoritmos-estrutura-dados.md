---
layout: post
title: "Unidade: Matemática Discreta, Algoritmos e Estruturas de Dados"
author: "Artur Cremonez"
description: "Explora como demonstrações matemáticas geram paradigmas de programação que conduzem a estruturas de dados."
categories: [matematica-discreta-finita, analise-projeto-algoritmos, estrutura-dados]
order: 1
lang: pt
---

# Pensamento discursivo (Dianóia)

O pensamento discursivo (aquele baseado em hipóteses) desempenha um papel central no mundo moderno, e o objetivo deste texto é mostrar como ele se manifesta na ciência da computação.

Como esse pensamento é baseado em hipóteses, vamos pensar a partir delas:  
Tendo uma hipótese, queremos extrair uma consequência (ainda que não seja imediatamente óbvia); essa consequência será chamada **tese**.

Desse modo,

**Se hipótese é verdadeira, então tese é verdadeira.**

**Computação:**  
Percebemos que essa é a mesma estrutura do `se` ... `então` ... da programação.

**Lógica formal:**  
Entretanto, a forma que definimos até aqui está confusa.  
Vamos repensar de maneira mais precisa...

A afirmação anterior pode ser reescrita como:  
"Não é o caso que uma hipótese é verdadeira e a tese é falsa", onde queremos que o valor-verdade seja 1 **quando provada**.

Formalmente:  
~(hipótese ^ ~tese), que é o mesmo que:  
**hipótese => tese**

Podemos assumir que a hipótese e a tese sejam quaisquer proposições, mas **a demonstração serve para garantir que a implicação será válida sempre que a hipótese for verdadeira.**

Daqui vem a necessidade da demonstração.

> Obs.: Isso que construímos até aqui é a forma lógica de um teorema, onde a demonstração é utilizada para garantir que essa implicação é, de fato, verdadeira.

Portanto, trataremos das formas de demonstração...

---

# Lógica da unidade

### Técnicas de demonstração

**Hipótese de indução:** assumir que funciona para um elemento genérico.  
**Passo indutivo:** utiliza-se a hipótese de indução para construir o próximo elemento.  
**Base:** mostrar que funciona para um elemento específico no início.

Assim, estamos dizendo que, se assumirmos que funciona para um elemento (hipótese de indução), então funcionará para o próximo (passo indutivo).

Ao mostrar que funciona para o início (base), criamos um efeito dominó.

Acaba, assim, funcionando para todos.

**Indução fraca:**  
Vem de uma "hipótese fraca" (mais restrita): assume que vale para o elemento imediatamente anterior (geralmente n−1) e, a partir disso, constrói o próximo.

**Indução forte:**  
Vem de uma "hipótese forte" (mais geral): assume que vale para todos os elementos menores que n, permitindo utilizar qualquer um deles para construir o próximo.

### Técnica de demonstração => Paradigma

Levando em conta a equivalência entre um teorema lógico (representado na forma hipótese => tese) e um `se` ... `então` ... da programação, tentaremos retirar, das duas técnicas de demonstração apresentadas, um paradigma de programação para cada.

### Paradigmas

Sendo a **hipótese de indução** assumir que funciona para um elemento, esse elemento será a função recursiva; estamos assumindo que ela retornou corretamente e funciona.

Sendo o **passo indutivo** utilizar a hipótese de indução para construir o próximo elemento, isso, no algoritmo, pode ser visto como o retorno da função, onde há a função recursiva (hipótese de indução) combinada com algo. Essa combinação representa o próximo valor.

Sendo a **base** mostrar que funciona para um elemento específico no início, na programação isso pode ser visto como `se` caso base `então` retorna o valor específico.

**Paradigma incremental:**  
Baseado na indução fraca, temos uma "hipótese fraca".  
Ou seja, a função chama a si mesma uma única vez, diminuindo o parâmetro da função recursiva de n para n−1.
```
INC(X, n)  
	Se n <= n0       // base
	Senão  
		Te(n)        // tempo da etapa atual  
		INC(X', n-1) // chamada recursiva (uso da hipótese de indução)  
		Tc(n)        // tempo de combinação (passo indutivo)
	fim-se
```

T(n) = T(n-1) + Te(n) + Tc(n)

**Paradigma divisão e conquista:**  
Baseado na indução forte, temos uma "hipótese forte".  
Ou seja, a função pode chamar a si mesma quantas vezes for necessário, diminuindo o parâmetro da função recursiva e, em cada recursão, abrangendo uma região de elementos.
```
D&C(X, 1, n)  
	Se n <= n0           // base
	Senão  
		Td(n)            // tempo de divisão
		D&C(X1, 1, n/2)  // chamada recursiva (uso da hipótese de indução) 
		D&C(X2, 1, n/2)  // chamada recursiva (uso da hipótese de indução) 
		Tc(n)            // tempo de combinação (passo indutivo)
	fim-se
```

T(n) = 2T(n/2) + Td(n) + Tc(n)

### Paradigma => Estrutura

A respeito do paradigma que se manifesta no modo de executar os dados, esse modo de execução cria uma estrutura em potência que se atualiza na estrutura de dados.

Assim, a estrutura de dados programada já existia em potência no paradigma.

Portanto, tentaremos retirar, dos dois paradigmas apresentados, suas respectivas estruturas de dados.

### A implementação da estrutura de dados pilha como resultado da estrutura geral do paradigma incremental (indução fraca)

A essência de uma pilha já está presente no paradigma incremental:

- LIFO (last in, first out) já está presente na pilha de recursão.  
- Estrutura linear já está presente no fato de o paradigma chamar a si mesmo uma única vez.

### A implementação da estrutura de dados lista como resultado da estrutura específica da busca linear do paradigma incremental (indução fraca)

Uma vez que utilizemos o caso específico do paradigma incremental, que é a busca linear, para acessar elementos em qualquer posição (incluindo adicionar um novo nó ou retirar um nó específico),

a busca linear opera sobre uma estrutura de dados linear, enquanto a execução continua sendo controlada por uma pilha (LIFO).

Entretanto, nesse caso, o comportamento sobre os dados deixa de ser de pilha e passa a ser de lista.

### A implementação da estrutura de dados árvore e seu percurso DFS como resultado da estrutura pura do paradigma divisão e conquista (indução forte)

A execução de um algoritmo de divisão e conquista, por meio da pilha de recursão, induz uma árvore de chamadas (árvore de recursão). Essa estrutura é percorrida naturalmente em profundidade (DFS), guiada pela própria pilha.

Para 2 chamadas recursivas, temos árvore binária.  
Para 3 chamadas recursivas, temos árvore ternária.  
...  
Para n chamadas recursivas, temos árvore n-ária.

---

# O problema dos algoritmos mortos

### Recapitulando a lógica

**Demonstração => Paradigma => Estrutura**

Será que essa lógica se manterá?

Problema: as demonstrações vindas da matemática discreta, embora úteis, parecem já chegar sem vida, pois tudo já está definido fora da execução.

### Invariantes como solução para as demonstrações estáticas da lógica

(Mostrar que invariantes não vêm da computação, mas que a computação dá vida a eles)

Problema: sistemas formais (e a programação está inclusa nisso) trabalham com definições estáticas. Como dar vida a algo assim?

Solução: usar algo estático (uma propriedade) para se manter durante a execução (algo vivo).

Assim,

É uma propriedade (escolhida para ser mantida durante a execução) que permanece verdadeira em todos os estados alcançáveis pela execução.

A execução percorre estados, refletindo a estrutura recursiva, enquanto o invariante é garantido por sua preservação a cada passo.

Sendo assim, iremos fazer uma demonstração indutiva do invariante.

Vamos ver, então, como seriam as demonstrações utilizando essa ideia.

### A criação de paradigmas baseados em execução

**=> Backtracking**

Como falado anteriormente da estrutura recursiva, iremos fazer uma demonstração indutiva do invariante.

Nesse novo método, nós juntaremos indução com demonstração por contradição.

**Agora, olhando apenas pela perspectiva da demonstração por contradição:**

Já que é preciso garantir, na execução, que o invariante se mantém, podemos fazer uma analogia com uma demonstração por redução ao absurdo.

> **Argumentos lógicos verdadeiros => Argumentos lógicos verdadeiros + contraexemplo (hipótese e negação da tese) => absurdo**

Problema: a saída sendo um absurdo não seria nada construtivo como um paradigma de programação.

Assim, temos:

> **invariante → possível violação local (contraexemplo local) → restauração do invariante**

A violação é um caminho da árvore de decisões que viola a restrição (invariante), e a restauração é, especificamente, o backtrack.

**=> Programação dinâmica**

Como falado anteriormente da estrutura recursiva, iremos fazer uma demonstração indutiva do invariante.

Nesse novo método, nós juntaremos indução com demonstração direta.

**Agora, olhando apenas pela perspectiva da demonstração direta:**

> **hipótese (entrada) => argumentos lógicos verdadeiros => tese (resposta)**  
> ou  
> **argumentos lógicos verdadeiros => argumentos lógicos verdadeiros + hipótese (entrada) => tese (resposta)**

A partir dessa forma, vamos pensar como ficaria a programação dinâmica...

invariante (definição feita pelo programador) → calcular, na execução, de modo que mantenha o invariante

**=> Outros paradigmas baseados na manutenção de invariantes**  
Esses paradigmas poderiam continuar sendo listados, mas para entender a lógica, esses já parecem suficientes.

### Balanceamento da árvore (AVL/Rubro-Negra) como resultado da estrutura do invariante

Durante a execução de operações que modificam a árvore (como inserção ou remoção), a árvore é percorrida implicitamente em profundidade (DFS), seguindo a estrutura de divisão e conquista induzida pela recursão.

Em cada nó, verifica-se se o **invariante de balanceamento** foi preservado. A quebra do invariante constitui um **contraexemplo local** à propriedade global da estrutura.

Esse contraexemplo dispara a necessidade de restaurar o invariante por meio de **transformações estruturais locais** (como rotações em árvores AVL ou Rubro-Negra).

Ou seja:

> **invariante → contraexemplo local → restauração do invariante**

Dessa forma, o balanceamento em árvores já estava presente, em potência, em um paradigma baseado na manutenção de invariantes.
