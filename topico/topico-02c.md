## [Tópico 02c] - Exercícios de revisão (3/3)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

<hr style="border:2px solid blue">

Para ilustrar as operações da SQL, considere o esquema lógico do BD Empresa.

<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Escreva em SQL as operações (1), (2) e (3).

|(1)|
|-|
|π <sub>Pnome, **F.Sexo**, Nome_dependente, **D.Sexo**</sub><br>&nbsp;&nbsp;&nbsp;&nbsp;(**ρ <sub>F</sub>** (FUNCIONARIO) ⨝ <sub>Cpf = Fcpf</sub> **ρ <sub>D</sub>** (DEPENDENTE))|

> SELECT F.Pnome, F.Sexo, D.Nome_dependente, D.Sexo
FROM (SELECT * FROM FUNCIONARIO) AS F
INNER JOIN (SELECT * FROM DEPENDENTE) AS D
ON F.Cpf = D.Fcpf;


|(2)|
|-|
|D(Cpf, Nome_dependente, Sexo_dependente) ←<br>&nbsp;&nbsp;&nbsp;&nbsp;π <sub>Fcpf, Nome_dependente, Sexo</sub> (DEPENDENTE)<br>RESULT ← π <sub>Pnome, Sexo, Nome_dependente, Sexo_dependente</sub> (FUNCIONARIO * D)|

> CREATE TABLE D AS
SELECT Fcpf AS Cpf, Nome_dependente, Sexo AS Sexo_dependente
FROM DEPENDENTE;

SELECT F.Pnome, F.Sexo, D.Nome_dependente, D.Sexo AS Sexo_dependente
FROM FUNCIONARIO F
INNER JOIN D
ON F.Cpf = D.Cpf;


|(3)|
|-|
|FUNC(**Cpf**, Pnome_func, Unome_func) ← <br>&nbsp;&nbsp;&nbsp;&nbsp;π <sub>**Cpf_supervisor**, Pnome, Unome</sub> (FUNCIONARIO)<br>SUPER ← π <sub>Cpf, Pnome, Unome</sub> (FUNCIONARIO)<br>RESULT ← π <sub>Pnome_func, Unome_func, Pnome, Unome (FUNC * SUPER)|

> CREATE TABLE FUNC AS
SELECT Cpf_supervisor AS Cpf, Pnome, Unome
FROM FUNCIONARIO;

CREATE TABLE SUPER AS
SELECT Cpf, Pnome, Unome
FROM FUNCIONARIO;

SELECT FUNC.Cpf, FUNC.Pnome_func, FUNC.Unome_func, SUPER.Pnome, SUPER.Unome
FROM FUNC
INNER JOIN SUPER
ON FUNC.Cpf = SUPER.Cpf;


<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Interprete as expressões a seguir (veja a tabela).

&#9888; (5 + NULL) = NULL<br>
&#9888; (5 < NULL) = NULL<br>
&#9888; (NULL <> NULL) = NULL<br>
&#9888; (NULL = NULL) = NULL<br>
&#9888; NULL OR TRUE =TRUE<br>
&#9888; NULL OR FALSE = NULL<br>
&#9888; NULL OR NULL = NULL<br>
&#9888; TRUE AND NULL = NULL<br>
&#9888; FALSE AND NULL = FALSE<br>
&#9888; NULL AND NULL = NULL<br>
&#9888; NOT NULL = = NULL

<img src="../media/fig-valor-nulo.jpg" width="450">

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Reescreva a junção abaixo sem usar a cláusula HAVING. Em seguida, transforme a solução em junção natural.

|(1)|
|-|
SELECT Pnome, Unome, COUNT(Fcpf)<br>FROM FUNCIONARIO JOIN DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;ON Cpf = Fcpf<br> GROUP BY Fcpf, Pnome, Unome<br>HAVING COUNT(Fcpf) >= 2|

> SELECT Pnome, Unome, COUNT(Fcpf)
FROM FUNCIONARIO
JOIN DEPENDENTE
ON Cpf = Fcpf
GROUP BY Pnome, Unome
HAVING COUNT(Fcpf) >= 2;

> SELECT P.Pnome, P.Unome, COUNT(F.Cpf)
FROM FUNCIONARIO F
NATURAL JOIN DEPENDENTE D
GROUP BY P.Pnome, P.Unome
HAVING COUNT(F.Cpf) >= 2;


<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> A construção abaixo é válida?

|(1)|
|-|
|SELECT Pnome, Unome, COUNT(Fcpf)<br>FROM FUNCIONARIO JOIN DEPENDENTE<br>&nbsp;&nbsp;&nbsp;&nbsp;ON Cpf = Fcpf<br> GROUP BY Fcpf, Pnome, Unome<br>HAVING COUNT(Fcpf) >= 2<br>ORDER BY 3 DESC, 1|

> A construção apresentada na consulta (1) é válida em SQL. Ela faz o seguinte:
 >   1. Seleciona as colunas Pnome, Unome e COUNT(Fcpf).
  >  2. Faz um INNER JOIN entre as tabelas FUNCIONARIO e DEPENDENTE usando a condição ON Cpf = Fcpf.
   > 3.Agrupa os resultados pelo Cpf, Pnome e Unome usando GROUP BY.
    >4. Filtra os grupos resultantes usando HAVING COUNT(Fcpf) >= 2, o que significa que serão retornados apenas os grupos com pelo menos duas ocorrências.
    >5. Ordena o resultado em ordem decrescente pelo terceiro item (COUNT(Fcpf)) e, em seguida, pelo primeiro item (Pnome).

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o propósito do comando abaixo?

|(1)|
|-|
|SELECT Salario FROM FUNCIONARIO WHERE Salario > <br>&nbsp;&nbsp;( SELECT MIN(Salario) FROM FUNCIONARIO WHERE Salario < <br>&nbsp;&nbsp;&nbsp;&nbsp;( SELECT MAX(Salario) FROM FUNCIONARIO WHERE Salario < <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;( SELECT MAX(Salario) FROM FUNCIONARIO ) ) )|

> Selecionar o salário de funcionários que têm salários maiores do que o segundo maior salário na tabela FUNCIONARIO, com execeção do menor salário. 

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o propósito dos comandos abaixo?

|(1)|(2)|(3)|
|-|-|-|
|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>EXCEPT<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>UNION<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf|SELECT S.Cpf, S.Pnome, S.Unome<br>FROM FUNCIONARIO AS F JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS S<br>&nbsp;&nbsp;&nbsp;&nbsp;ON F.Cpf_supervisor = S.Cpf<br>INTERSECT<br>SELECT F.Cpf, F.Pnome, F.Unome <br>FROM DEPENDENTE AS D JOIN<br>&nbsp;&nbsp;&nbsp;&nbsp;FUNCIONARIO AS F<br>&nbsp;&nbsp;&nbsp;&nbsp;ON D.Fcpf = F.Cpf|

> (1) EXCEPT (Diferença):
O comando (1) retorna funcionários que supervisionam outros funcionários (indicado pelo JOIN entre FUNCIONARIO e FUNCIONARIO com base na relação de supervisor) e que não são dependentes de outros funcionários (indicado pelo EXCEPT, que subtrai os funcionários que também são dependentes). Em resumo, ele retorna os supervisores que não são dependentes.

>(2) UNION (União):
O comando (2) retorna uma lista que combina dois conjuntos de resultados diferentes. A primeira parte do UNION retorna os funcionários que supervisionam outros funcionários, enquanto a segunda parte retorna os funcionários que são dependentes. Em resumo, ele retorna uma lista que inclui tanto supervisores quanto dependentes em um único resultado.

>(3) INTERSECT (Interseção):
O comando (3) retorna funcionários que supervisionam outros funcionários (indicado pelo JOIN entre FUNCIONARIO e FUNCIONARIO com base na relação de supervisor) e que também são dependentes de outros funcionários (indicado pelo INTERSECT, que retorna os funcionários que estão presentes em ambos os conjuntos de resultados). Em resumo, ele retorna funcionários que são tanto supervisores quanto dependentes.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Os comandos em (1) e (2) são equivalentes?

|(1)|(2)|
|-|-|
|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE EXISTS (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT \***<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPENDENTE**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE FUNCIONARIO.Cpf = DEPENDENTE.Fcpf** )|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE EXISTS (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT NULL**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPENDENTE**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE FUNCIONARIO.Cpf = DEPENDENTE.Fcpf** )|
|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE Cpf IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Cpf_gerente**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPARTAMENTO** )|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE Cpf = (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Cpf_gerente**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM DEPARTAMENTO** )|

> (1): Retorna funcionários com pelo menos um dependente.

> (2): Também retorna funcionários com pelo menos um dependente, mas é mais eficiente em termos de desempenho.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> A consulta abaixo poderia retornar _tuplas_ iguais?

|(1)|
|-|
|SELECT Pnome, Unome<br>FROM FUNCIONARIO<br>WHERE (Cpf, 10) IN<br>&nbsp;&nbsp;&nbsp;&nbsp;( **SELECT Fcpf, Horas**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM TRABALHA_EM** )|

>Sim, a consulta (1) pode retornar tuplas iguais se houver entradas duplicadas na tabela TRABALHA_EM com o mesmo valor de Cpf e Horas.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o propósito do comando abaixo?

|(1)|
|-|
|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO AS EXTERNA<br>WHERE NOT EXISTS (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT \***<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS INTERNA**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE INTERNA.Salario < EXTERNA.Salario** )|

> Retorna funcionários com o salário mais alto na tabela FUNCIONARIO.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o propósito do comando abaixo?

|(1)|
|-|
|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO AS EXTERNA<br>WHERE 3 > (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT COUNT(DISTINCT Salario)**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS INTERNA**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE INTERNA.Salario > EXTERNA.Salario** )|

> Retorna funcionários cujos salários estão entre os três maiores salários distintos na tabela FUNCIONARIO.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Os comandos em (1), (2) e (3) são equivalentes?

|(1)|(2)|(3)|
|-|-|-|
|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario >= ALL  (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO** )|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO<br>WHERE Salario NOT IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE Salario < ANY (**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO )** )|SELECT Pnome, Unome, Salario<br>FROM FUNCIONARIO AS EXTERNA<br>WHERE Salario NOT IN (<br>&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS CENTRAL**<br>&nbsp;&nbsp;&nbsp;&nbsp;**WHERE EXISTS (**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**SELECT Salario**<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FROM FUNCIONARIO AS INTERNA<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WHERE CENTRAL.Salario < INTERNA.Salario )** )|

> Não, os comandos em (1), (2) e (3) não são equivalentes. Eles têm propósitos diferentes:
> (1): Retorna funcionários com o salário igual ou superior a todos os salários na tabela FUNCIONARIO.
> (2): Retorna funcionários cujos salários não estão na lista de salários menores que qualquer salário na tabela FUNCIONARIO.
> (3): Retorna funcionários cujos salários não estão na lista de salários menores que pelo menos um salário de outros funcionários na tabela FUNCIONARIO, considerando uma subconsulta com aliases (EXTERNA, CENTRAL e INTERNA) para fazer essa comparação.

<hr style="border:2px solid blue">
