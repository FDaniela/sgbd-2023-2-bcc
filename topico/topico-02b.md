## [Tópico 02b] - Exercícios de revisão (2/3)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

<hr style="border:2px solid blue">

Para ilustrar as operações da álgebra relacional, considere o esquema lógico do BD Empresa.

<img src="../media/fig-esquema-logico-bdempresa.jpg" width="450">

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual a distinção entre as expressões abaixo?

|Expressão 1|Expressão 2|
|-|-|
|FUNC_F ← σ <sub>Sexo='F'</sub> (FUNCIONARIO)<br>π <sub>Cpf, Pnome, Unome</sub> (FUNC_F)|FUNC_F ← σ <sub>Sexo='F'</sub> (FUNCIONARIO)<br>π <sub>'Feminino', Pnome, Unome, 10, NULL</sub> (FUNC_F)|

>**Expressão 1:**
>- Seleciona todas as funcionárias (Sexo='F') da tabela FUNCIONARIO.
>- Projeta (retira) as colunas "Cpf", "Pnome", e "Unome" da tabela resultante e cria uma nova relação chamada FUNC_F.

>**Expressão 2:**
>- Seleciona todas as funcionárias (Sexo='F') da tabela FUNCIONARIO.
>- Projeta (retira) as colunas "Pnome" e "Unome" da tabela resultante.
>- Adiciona colunas com valores constantes ('Feminino', 10, e NULL) à relação resultante.
>- Cria uma nova relação chamada FUNC_F.

>A principal diferença está na projeção dos resultados na Expressão 2, onde são adicionadas colunas com valores constantes.


<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o retorno da expressão abaixo?

|Expressão|
|-|
|π SUPERVISIONADO.Pnome, SUPERVISOR.Pnome ( ρ SUPERVISIONADO (FUNCIONARIO) &#8904;<sub>SUPERVISIONADO.Cpf_supervisor = SUPERVISOR.CPF</sub> ρ SUPERVISOR (FUNCIONARIO) )|

> Para cada empregado o nome do seu supervisor será dado.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> As afirmações abaixo são verdadeiras?

[V] R ∪ S = S ∪ R ?<br>
[V] R ∩ S = S ∩ R ?<br>
[F] R - S = S - R ?<br>
[V] R ∪ (S ∪ T ) = (R ∪ S) ∪ T ?<br>
[V] (R ∩ S) ∩ T = R ∩ (S ∩ T) ?<br>

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o conteúdo de RESULT1, RESULT2 e RESULT3?

|Expressão|Operação|
|-|-|
|**FUNC_DEPTO_5 ← σ<sub>Dnr=5</sub>(FUNCIONARIO)**|RENOMEAÇÃO, SELEÇÃO|
|**TRABALHA_DEPTO_5 ← π<sub>Cpf</sub>(FUNC_DEPTO_5)**|RENOMEAÇÃO, PROJEÇÃO|
|**SUPERVISIONA_DEPTO_5(Cpf) ← π<sub>Cpf_supervisor</sub>(FUNC_DEPTO_5)**|RENOMEAÇÃO, PROJEÇÃO|
|**RESULT1 ← TRABALHA_DEPTO_5 ∪ SUPERVISIONA_DEPTO_5**|RENOMEAÇÃO, ***UNIÃO***|
|**RESULT2 ← TRABALHA_DEPTO_5 ∩ SUPERVISIONA_DEPTO_5**|RENOMEAÇÃO, ***INTERSEÇÃO***|
|**RESULT3 ← TRABALHA_DEPTO_5 - SUPERVISIONA_DEPTO_5**|RENOMEAÇÃO, ***DIFERENÇA***|

>- RESULT1 contém todas as linhas de TRABALHA_DEPTO_5 e SUPERVISIONA_DEPTO_5, sem duplicatas.
>- RESULT2 contém apenas as linhas presentes tanto em TRABALHA_DEPTO_5 quanto em SUPERVISIONA_DEPTO_5.
>- RESULT3 contém todas as linhas de TRABALHA_DEPTO_5 que não estão em SUPERVISIONA_DEPTO_5.
>-------------------------
> “Todo supervisor é um funcionário, mas nem todo funcionário é um supervisor”<br>
> FUNC_DEPTO_5: todos os funcionários do departamento 5<br>
> TRABALHA_DEPTO_5: todos os CPFs dos funcionários do departamento 5<br>
> SUPERVISIONA_DEPTO_5: todos os CPFs dos supervisores do departamento 5<br>
> RESULT1: os CPFs de todos os funcionários do departamento 5<br>
> RESULT2: os CPFs dos supervisores do departamento 5<br>
> RESULT3: os CPFs dos funcionários do departamento 5 que não são supervisores<br>


<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> No contexto das operações UNIÃO, INTERSEÇÃO e DIFERENÇA, para comparar as _tuplas_ presentes nas duas relações de entrada, as _tuplas_ devem ser do mesmo tipo, ou seja, deve haver 'compatibilidade de união' (compatibilidade de tipo). O que significa 'compatibilidade de união'?

> "Compatibilidade de união" significa que as duas relações de entrada devem ter o mesmo número e tipo de colunas para que as operações de união, interseção e diferença possam ser realizadas corretamente.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Qual o conteúdo de RESULT?

|Expressão|Operação|
|-|-|
|**SILVA ← σ<sub>Pnome=‘João’ AND Unome=‘Silva’</sub>(FUNCIONARIO)**|RENOMEAÇÃO, SELEÇÃO|
|**PROJETOS_SILVA ← π<sub>Pnr</sub>(TRABALHA_EM ⋈<sub>Fcpf=Cpf</sub>SILVA)**|RENOMEAÇÃO, EQUIJUNÇÃO, PROJEÇÃO|
|**PROJETOS_TODOS_FUNCS ← π<sub>Fcpf, Pnr</sub>(TRABALHA_EM)**|RENOMEAÇÃO, PROJEÇÃO|
|**FUNC(Cpf) ← PROJETOS_TODOS_FUNCS ÷ PROJETOS_SILVA**|RENOMEAÇÃO, ***DIVISÃO***|
|**RESULT ← π<sub>Pnome, Unome</sub>(FUNC * FUNCIONARIO)**|RENOMEAÇÃO, JUNÇÃO NATURAL, PROJEÇÃO|

>  O conteúdo de RESULT será uma tabela que contém os nomes (Pnome e Unome) dos funcionários cujo "Cpf" está na tabela FUNC (resultado da divisão) e que atendem às condições de seleção inicial (primeira operação). O conteúdo exato dependerá dos dados reais nas tabelas e das condições especificadas nas operações.

<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Que expressão da álgebra relacional implementa as seguintes consultas?

1. Qual o nome dos funcionários que possuem dois ou mais dependentes?

|Expressão|Operação|
|-|-|
|RESUMO(Cpf, Qtde_depend)← <sub>Fcpf</sub> ℑ CONTA <sub>Fcpf</sub> (DEPENDENTE)|RENOMEAÇÃO, ***FUNÇÃO AGREGADA***, ***AGRUPAMENTO***|
|RESUMO_DOIS ← σ <sub>Qtde_depend >= 2</sub> (RESUMO)|RENOMEAÇÃO, SELEÇÃO|
|RESULT ← π <sub>Pnome, Unome</sub> (RESUMO_DOIS * FUNCIONARIO)|RENOMEAÇÃO, JUNÇÃO NATURAL, PROJEÇÃO|

>RESULT ← π <sub>Pnome, Unome</sub> (σ <sub>Qtde_depend >= 2</sub> (FUNCIONARIO ⨝ <sub>Cpf</sub> DEPENDENTE))


2. Qual o nome dos funcionários, que possuem mais de um dependente e trabalham em mais de um projeto?
>RESULT ← π <sub>Pnome, Unome</sub> (σ <sub>Qtde_depend > 1 AND Qtde_projetos > 1</sub> (FUNCIONARIO ⨝ <sub>Cpf</sub> DEPENDENTE ⨝ <sub>Cpf</sub> TRABALHA_EM))


3. Qual o nome dos dependentes, cujo funcionário responsável pelo dependente é supervisor direto de mais de um funcionário?
>RESULT ← π <sub>Pnome_dependente, Unome_dependente</sub> (σ <sub>SupervisorQtde > 1</sub> (DEPENDENTE ⨝ <sub>Cpf_dependente = Cpf</sub> FUNCIONARIO))



<hr style="border:2px solid blue">

#### <ins>EXERCÍCIO:</ins> Uma lista de comandos, categorizados por dificuldade ...

> **Grupo 1: Fácil**

1. π <sub>Pnome, Unome</sub> (FUNCIONARIO)
1. σ <sub>Sexo="F"</sub> (FUNCIONARIO)
1. π <sub>Pnome, Unome</sub> ( σ <sub>Sexo="F"</sub> (FUNCIONARIO) )
1. TRABALHA_EM  X  PROJETO
1. ρ FUNC ( π Pnome, Unome ( σ <sub>Sexo="F"</sub> (FUNCIONARIO) ) )
1. ρ FUNC (PrimeiroNome, UltimoNome) ( π <sub>Pnome, Unome</sub> ( σ <sub>Sexo="F"</sub> (FUNCIONARIO) ) )

> **Grupo 2: Simples**

1. TRABALHA_EM &#8904; <sub>Fcpf=cpf</sub>  FUNCIONARIO
1. FUNCIONARIO * ( ρ (cpf, Projnumero, Horas) (TRABALHA_EM) * PROJETO )
1. π <sub>Cpf_supervisor</sub> (FUNCIONARIO) ∪ π <sub>Cpf_gerente</sub> (DEPARTAMENTO)
1. π <sub>Cpf_supervisor</sub> (FUNCIONARIO) ∩ π <sub>Cpf_gerente</sub> (DEPARTAMENTO)
1. π <sub>Cpf_supervisor</sub> (FUNCIONARIO) – π <sub>Cpf_gerente</sub> (DEPARTAMENTO)
1. SUP_E_GER ← ρ (cpf) ( π Cpf_supervisor (FUNCIONARIO) ∩ π Cpf_gerente (DEPARTAMENTO) )<br>RESP ← π Pnome, Unome ( FUNCIONARIO * SUP_E_GER )
1. π Cpf, Pnome (FUNCIONARIO) ∩ π Fcpf, Nome_dependente (DEPENDENTE)
1. SUP_SEM_DEP ← ρ (cpf) ( π Cpf_supervisor (FUNCIONARIO) – π Fcpf (DEPENDENTE) )<br>RESP ← π Pnome, Unome ( FUNCIONARIO * SUP_SEM_DEP )

> **Grupo 3: Motivador**

1. ℑ CONTA cpf , MÉDIA Salario, MÁXIMO Salario , MÍNIMO Salario (FUNCIONARIO)
1. Dnr ℑ CONTA cpf , MÉDIA Salario, MÁXIMO Salario , MÍNIMO Salario (FUNCIONARIO)
1. ρ RESUMO (Dnumero, QtdeFunc, MediaSalario) Dnr ℑ CONTA cpf , MÉDIA Salario (FUNCIONARIO)<br>RESPOSTA ← π Dnome, QtdeFunc, MediaSalario ( DEPARTAMENTO * RESUMO )
1. ρ SUPERVISOR (π Cpf, Pnome (FUNCIONARIO) )<br>FUNCIONARIO &#8904; <sub>FUNCIONARIO.Cpf_supervisor = SUPERVISOR.Cpf</sub> SUPERVISOR
1. ρ SUPERVISOR (Cpf_supervisor, Pnome_supervisor) (π Cpf, Pnome (FUNCIONARIO) )<br>π Pnome, Pnome_supervisor (FUNCIONARIO * SUPERVISOR ) 

> _Under Construction_ ...

<hr style="border:2px solid blue">
