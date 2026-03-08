---
layout: post
title: "SQL Parte 01: Desconstruindo a Lógica das Consultas Relacionais"
description: "Exploração prática de SQL e álgebra relacional, conectando teoria e exemplos com SELECT, WHERE, GROUP BY e JOIN."
categories: [banco-de-dados, sql, aprendizado]
tags: [sql, álgebra-relacional, banco-de-dados, tutorial]
author: "Artur Cremonez"
---

# Introdução

Para entender **bancos de dados relacionais**, podemos começar pela forma mais simples de representar os dados: **uma tabela**.

Tabelas são uma forma extremamente comum de organizar informações. Mesmo quem não trabalha com tecnologia já teve contato com esse tipo de estrutura, seja em uma planilha do **Excel**, seja em uma tabela impressa em uma prancheta ou formulário.

As tabelas organizam dados em **linhas** e **colunas**. Cada linha representa um **registro**, enquanto cada coluna representa um **tipo específico de informação**.

Mas surge então uma pergunta:

**Por que usar tabelas?**

A principal razão é **consultar os dados que estão nelas**, organizando as informações de forma que seja fácil localizar e analisar os registros.

Para isso, bancos de dados relacionais utilizam uma linguagem chamada **SQL (Structured Query Language)**. Dentro do SQL, existe um conjunto de comandos voltados especificamente para consultas de dados, frequentemente chamado de **DQL (Data Query Language)**.

Neste texto, vamos explorar algumas das operações fundamentais que uma linguagem de consulta precisa oferecer para trabalhar com tabelas, mas essas operações não surgem do nada: elas derivam das necessidades que a própria tabela impõe.

---

# Tese: Análise da tabela individual

Vamos começar analisando o caso mais simples: uma tabela individual.
- Em relação às **colunas**, para consultas é necessário **reduzir as colunas**, retornando uma tabela que **exclui determinadas colunas**.
- Em relação às **linhas**, para consultas é necessário **reduzir as linhas**. Isso pode ocorrer de duas formas principais: **filtrando linhas**, cujo valor em determinada coluna não atende a uma condição definida pelo programador, ou **agrupando linhas que possuem valores repetidos em uma coluna**, de modo que cada grupo passe a ser representado por um único resultado.
- Além disso, para obter a linha com o **menor ou maior valor** de uma coluna, é necessário **ordenar os dados** e, caso desejado, **limitar o resultado** às n primeiras linhas.

## 1º - Retornar uma tabela reduzindo as colunas, retornando uma tabela que exclui determinadas colunas

Exemplo:

Se temos uma tabela com as colunas **coluna_a**, **coluna_b** e **coluna_c**, e queremos retornar uma nova tabela apenas com **coluna_a** e **coluna_b**, podemos imaginar isso em pseudocódigo da seguinte maneira:

```
Para cada linha em Tabela faça
pegar linha.coluna_a
pegar linha.coluna_b
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Dado esse padrão, o SQL pode criar um comando que faça exatamente isso:

```
SELECT coluna_a, coluna_b
FROM Tabela
```

### Problema

E se a tabela sobre a qual queremos fazer o `SELECT` já for **resultado de outra consulta**?

### Solução

Podemos colocar essa consulta entre parênteses no local onde normalmente estaria o nome da tabela. Por exemplo:

```
SELECT ...
FROM (SELECT ...)
```

Isso é chamado de **subquery** (ou **subconsulta**).

Pensar nessa operação como um processo que percorre cada linha da tabela pode ser mentalmente confuso.

Uma alternativa é **abstrair a tabela**, mantendo sua forma, mas ignorando seu conteúdo específico.

Ao fazer isso, chegamos a um operador formal da **álgebra relacional** chamado **projeção**, representado pela letra grega **π**.

No exemplo, isso pode ser representado da seguinte maneira:

π(coluna_a, coluna_b)(Tabela)

### Conclusão

Partimos da **tabela**
Passamos pelo **pseudocódigo**
Chegamos ao **SQL**
E finalmente chegamos à **Álgebra Relacional**.

Ou seja, partindo de algo **concreto comum**, chegamos a uma **abstração formal**.

Curiosamente, a ordem em que apresentamos os conceitos aqui é o **oposto de como um programador pensa** (afinal, a abstração responsável por ignorar o que não é necessário é a mesma que nos dá clareza de pensamento) e, por consequência, também é **oposta à ordem em que foi desenvolvida historicamente**.

Na prática, partir do **abstrato** pode ajudar a obter maior clareza de pensamento; no entanto, também é uma maneira dogmática de se pensar, o que pode atrapalhar o aprendizado real.

Agora, precisamos verificar se essa mesma lógica se manterá para os próximos operadores...

## 2º - Retornar uma tabela reduzindo as linhas filtrando linhas cujo valor em determinada coluna não atende a uma condição definida (condicionais)

```
Para cada linha em Tabela faça
Se linha.coluna_x operador_relacional valor então
retornar linha
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Dado esse padrão, o SQL pode criar um comando que faça exatamente isso:

```
SELECT *
FROM Tabela
WHERE coluna_x operador_relacional valor
```

**Abstraindo a tabela**, mantendo sua forma, mas ignorando seu conteúdo específico.

Ao fazer isso, chegamos a um operador formal da **álgebra relacional** chamado **seleção**, representado pela letra grega **σ**.

No exemplo, isso pode ser representado da seguinte maneira:

σ(coluna_x operador valor)(Tabela)

## 3º - Retornar uma tabela reduzindo as linhas agrupando linhas que possuem valores repetidos em uma coluna

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        ...
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Dado esse padrão, o SQL pode criar um comando que faça exatamente isso:

```GROUP BY coluna_x```

Problema:
Agrupar a coluna com os valores repetidos, mas e as outras colunas com valores diferentes?

Solução:
Para isso, é necessário utilizar **funções de agregação**, aplicadas às colunas com valores diferentes de modo que, quando o agrupamento ocorrer, seja inserido um único valor para cada linha dessas colunas.

Sendo assim, quais seriam as funções que recebem uma lista de valores e retornam um único valor?

### COUNT

Conta quantos registros existem em cada grupo.

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x
contador = 0

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        contador++
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL:

```
SELECT coluna_x, COUNT(coluna_y)
FROM Tabela
GROUP BY coluna_x
```

### SUM

Soma os valores de uma coluna dentro de cada grupo.

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x
soma = 0

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        soma = soma + linha.coluna_y
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL:

```
SELECT coluna_x, SUM(coluna_y)
FROM Tabela
GROUP BY coluna_x
```

### AVG

Calcula a média dos valores.

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x
soma = 0
contador = 0

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        soma = soma + linha.coluna_y
        contador++
    
media = soma / contador
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL:

```
SELECT coluna_x, AVG(coluna_y)
FROM Tabela
GROUP BY coluna_x
```

### MIN

Retorna o menor valor do grupo.

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x
min_valor = infinito

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        Se linha.coluna_y < min_valor então
            min_valor = linha.coluna_y
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL:

```
SELECT coluna_x, MIN(coluna_y)
FROM Tabela
GROUP BY coluna_x
```

### MAX

Retorna o maior valor do grupo.

```
Para cada linha em Tabela faça
grupo <- linha.coluna_x
max_valor = -infinito

Para cada linha em Tabela faça
    Se linha.coluna_x == grupo então
        Se linha.coluna_y > max_valor então
            max_valor = linha.coluna_y
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL:

```
SELECT coluna_x, MAX(coluna_y)
FROM Tabela
GROUP BY coluna_x
```

**Abstraindo a tabela**, mantendo sua forma, mas ignorando seu conteúdo específico.

Ao fazer isso, chegamos a um operador formal da **álgebra relacional** chamado **agregação**, representado pela letra grega **γ**.

No exemplo, isso pode ser representado da seguinte maneira:

γ(coluna_x; função_agregação(coluna_y))(Tabela)

## 4º - Ordenação

Seguindo a ideia de que, para retornar a linha de menor ou maior valor de uma coluna, é necessário **ordenar** e **limitar** os resultados às n primeiras linhas.

Então, devemos fazer a parte de **ordenar os dados retornados pela consulta**.

Podemos pensar nessa operação como a aplicação de **algoritmos de ordenação** estudados em lógica de programação, como, por exemplo:

- Bubble Sort
- Merge Sort
- Quick Sort

*Nota: esses algoritmos são exemplos simplificados e não capturam a complexidade real de um banco de dados real.*

Em SQL, isso é feito com:

```ORDER BY coluna_z ASC```

ou

```ORDER BY coluna_z DESC```

Essa operação não existe na Álgebra Relacional, pois ela é um sistema formal, e a ênfase está na forma. A operação nada altera na forma (modificando estruturalmente a tabela), e sim o conteúdo (organizando os dados).

## 5º - Limitar resultados

Seguindo a ideia de que, para retornar a linha de menor ou maior valor da coluna, é necessário ordenação e limitar para ter apenas as n primeiras linhas.

Então, vamos fazer a parte de retornar **apenas uma parte dos resultados**.

```
contador = 0

Para cada linha em Tabela faça
Se contador < n então
retornar linha
contador++
```
> *Nota: pseudocódigo simplificado; complexidade real não incluída.*

Em SQL, isso é feito com:

LIMIT n

Como uma operação dependente de outra que não possui correspondente na Álgebra Relacional (`ORDER BY`), já podemos imaginar que não existe correspondente também para esta (`LIMIT n`).

---
# Antítese: Análise das Tabelas

Terminamos a análise da **tabela individual**, mas e quando trabalhamos com **tabelas em relação com outras tabelas**? Será que a lógica apresentada até agora se mantém?

Pensando em **combinar tabelas**, podemos observar que isso pode acontecer de duas formas principais:

- **Juntar tabelas em relação às linhas**
- **Juntar tabelas em relação às colunas**

Como já analisamos as operações possíveis em uma tabela individual, podemos agora **abstrair a forma da tabela e pensar apenas em seu conteúdo**. Matematicamente, isso corresponde ao conceito de **conjunto**: uma coleção bem definida de objetos, elementos ou números.

Problema: conjuntos precisam de elementos de mesmo tipo para serem conjuntos.

Sendo assim:

A tabela pode ser conjunto em relação aos dados de cada coluna da tabela?
R: Não, em cada coluna há um tipo de dado diferente, o que faria com que fosse um conjunto de elementos de tipos diferentes e isso não poderia ser um conjunto.

A tabela pode ser conjunto em relação a cada célula da tabela?
R: Não, pois cada célula está associada a uma coluna (com seu tipo específico de dados), sendo assim teríamos grupos de elementos com tipos de dados distintos e isso não poderia ser um conjunto.

A tabela pode ser conjunto em relação a cada linha da tabela?
R: Sim, pois a cada linha está associada às mesmas colunas (tipos de dados), criando assim um tipo único de dados em cada linha. Desse modo, teríamos um conjunto com elementos homogêneos (como os conjuntos precisam ser).

Problema: conjuntos precisam de elementos únicos para serem conjuntos.

Portanto, é necessário que exista ao menos uma coluna cujo valor seja único em cada linha. Podem existir mais de uma coluna que garanta essa condição; o nome dessas colunas é **Chave Candidata**, e a escolhida para satisfazer isso será chamada de **Chave Primária**.

A partir dessa perspectiva, podemos entender melhor como as operações entre tabelas funcionam.

## Juntar tabelas em relação às linhas

Quando pensamos em tabelas como conjuntos de tuplas (linhas), podemos aplicar diretamente operações da teoria dos conjuntos. Algumas dessas operações comuns são:

- **União (UNION)** – combina as tuplas de duas tabelas.
```
SELECT coluna_1, coluna_2
FROM TabelaA
UNION
SELECT coluna_1, coluna_2
FROM TabelaB;
```

- **Interseção (INTERSECT)** – retorna apenas as tuplas que existem nas duas tabelas.
```
SELECT coluna_1, coluna_2
FROM TabelaA
INTERSECT
SELECT coluna_1, coluna_2
FROM TabelaB;
```

- **Diferença (EXCEPT)** – retorna as tuplas que existem em uma tabela, mas não na outra.
```
SELECT coluna_1, coluna_2
FROM TabelaA
EXCEPT
SELECT coluna_1, coluna_2
FROM TabelaB;
```

Essas operações funcionam **comparando linhas entre tabelas (ou conjuntos de tuplas)**, exatamente como na Álgebra Relacional, mas usando a sintaxe prática do SQL.

## Juntar tabelas em relação às colunas

Outra forma de combinar tabelas é **ligando suas colunas**, criando novas tuplas que combinam informações de duas tabelas diferentes.

A base dessas operações é o **produto cartesiano**.

O produto cartesiano combina **cada linha de uma tabela com todas as linhas da outra tabela**.

Problema: se cada linha precisa ser associada a uma linha específica da outra tabela, o produto cartesiano puro **não garante essa correspondência**, pois combina todas as linhas de uma tabela com todas as linhas da outra.

Para associar cada linha de uma tabela a uma linha específica de outra tabela de forma única, usamos uma **chave estrangeira**.
Essa chave é uma coluna na tabela atual que armazena o valor da **chave primária** da tabela referenciada, garantindo que cada associação seja **única e consistente**.

Dessa forma, podemos imaginar um **produto cartesiano** entre as duas tabelas, mas **mantendo apenas as linhas em que a chave primária da tabela referenciada coincide com a chave estrangeira da tabela atual**.

A Álgebra Relacional utiliza esse conceito como ponto de partida para formalizar operadores de combinação de tabelas, como o **JOIN**.

### INNER JOIN  (A⋈B)

Retorna apenas as linhas que possuem **correspondência em ambas as tabelas**.

```
SELECT A.coluna_1, B.coluna_2
FROM A
INNER JOIN B
ON A.chave_primaria = B.chave_estrangeira;
```

### LEFT JOIN  (A⟕B)

Retorna **todas as linhas da tabela da esquerda (A)** e apenas as correspondentes da tabela da direita (B).

Quando não há correspondência, os valores da tabela da direita (B) aparecem como `NULL`.

```
SELECT A.coluna_1, B.coluna_2
FROM A
LEFT JOIN B
ON A.chave_primaria = B.chave_estrangeira;
```

### RIGHT JOIN  (A⟖B)

Retorna **todas as linhas da tabela da direita (B)** e apenas as correspondentes da tabela da esquerda (A).

Quando não há correspondência, os valores da tabela da esquerda (A) aparecem como `NULL`.

```
SELECT A.coluna_1, B.coluna_2
FROM A
RIGHT JOIN B
ON A.chave_primaria = B.chave_estrangeira;
```

### FULL JOIN  (A⟗B)

Retorna **todas as linhas de ambas as tabelas**, combinando as que possuem correspondência e preenchendo as demais com `NULL`.

```
SELECT A.coluna_1, B.coluna_2
FROM A
FULL JOIN B
ON A.chave_primaria = B.chave_estrangeira;
```

---

# Síntese: Conclusão

Analisando a tabela como conjunto, percebemos que a tabela, na perspectiva da Álgebra Relacional, sempre foi conjunto de tuplas. Mesmo para os operadores provenientes da análise da tabela individual, percebemos que esses operadores podem ser vistos como operações realizadas em um conjunto.

Isso porque, sendo uma tabela um conjunto de tuplas, e sendo uma consulta em SQL uma tabela resultante, teremos como resultado outro conjunto de tuplas. Então, o que esses operadores fazem é, a partir de um conjunto de tuplas, criar outro conjunto de tuplas resultante.

Desse modo, temos que...

**Para a projeção (representado pela letra π) ou ```SELECT``` em SQL:**
Sendo uma tabela um conjunto de tuplas, a projeção seleciona os atributos (colunas) desejados e, a partir disso, cria uma nova tabela que é um novo conjunto de tuplas.

**Para a seleção (representado pela letra σ) ou ```WHERE``` em SQL:**
Aqui, retornamos apenas o **subconjunto de tuplas (linhas) que satisfazem uma condição**. A operação filtra o conjunto original, reduzindo o número de tuplas, mas mantendo todos os atributos (colunas).

**Para agregação (representado pela letra γ) ou ```GROUP BY``` em SQL + funções de agregação:**
A agregação cria subconjuntos **temporários de tuplas** para cada valor distinto das colunas agrupadas. Em seguida, aplica **funções de agregação** (como COUNT, SUM, AVG, MIN, MAX) a esses subconjuntos.
O resultado final é uma **nova tabela (ou conjunto de tuplas) agregada**, formada a partir dos valores calculados em cada grupo.

> *Nota prática:*
> *No SQL, é possível que a mesma linha apareça mais de uma vez, ao contrário do modelo teórico da **Álgebra Relacional**, onde cada tupla é única.*
> *No entanto, se um bom programador seguir a lógica da álgebra relacional e usar corretamente a **chave primária**, cada linha será única, respeitando o conceito de conjunto de tuplas.*
> *Os operadores que mostramos (`SELECT`, `WHERE`, `GROUP BY`) são inspirados na álgebra relacional, mas o SQL real vai além da teoria. Ele combina:*
> *- Álgebra Relacional*
> *- Cálculo Relacional*
> *- Extensões práticas necessárias para aplicações reais*
> *Além disso, SQL adiciona recursos que não existem na álgebra pura, como:*
> *- Ordenação*
> *- Limite de resultados*
> *- Funções agregadas complexas*
> *- Suporte a valores `NULL`*
> *Portanto, SQL é **uma linguagem pragmática baseada na teoria**, e não uma derivação direta e pura da Álgebra Relacional.*
