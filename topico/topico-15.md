## [Tópico 15] - Estruturas de indexação (3/5)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*


### 3. <ins>ÍNDICE DE AGRUPAMENTO (_CLUSTERING INDEX_)</ins>

**IMPORTANTE.**<br>
Um <ins>índice de agrupamento</ins> (_clustering index_) pode ser criado para um arquivo de dados quando:
- O arquivo de dados é um <ins>arquivo ordenado</ins>.
- O <ins>campo de ordenação</ins> do arquivo de dados <ins>não possui a restrição de valor único</ins> para cada registro:
  - o campo de ordenação não é chave de ordenação;
  - o campo de ordenação é também referenciado como <ins>campo de agrupamento</ins>;
  - ou seja, os registros do arquivo de dados estão fisicamente ordenados com respeito a um campo não-chave.
- <ins>O campo de indexação do arquivo do índice é o campo de ordenação do arquivo de dados</ins>.

Um <ins>**índice de agrupamento**</ins> é um <ins>arquivo ordenado</ins>:
- É um arquivo com registros de comprimento fixo.
- Cada registro possui com dois campos:
  - um <ins>valor</ins> [com o mesmo domínio] do campo de ordenação;
  - um ponteiro para um bloco de disco (um endereço de bloco do arquivo de dados).
- Dessa forma, cada <ins>entrada (registro)</ins> na posição **i** (i-ésimo registro do arquivo):
  - **< K(i), P(i) >**, onde:
    - **_K(i)_** é um valor do campo de indexação,
    - **_P(i)_** é um endereço físico de um bloco (ou página) no arquivo de dados.

A figura abaixo ressalta um <ins>índice de agrupamento</ins> (à esq.):<br>
&#x267B; O campo de ordenação (campo de agrupamento) é _Dept_number_.<br>
&#x267B; Há vários registros de dados (à dir.) para um mesmo valor de _Dept_number_.<br>
&#x267B; Há uma entrada no índice de agrupamento (à esq) para cada valor distinto do campo de agrupamento [no arquivo de dados].<br>
&#x267B; Cada valor de ponteiro de bloco (à esq.):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... referencia o <ins>primeiro bloco de dados</ins> (à dir.) que tem registro(s) com o valor do campo de agrupamento;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... por exemplo, registros com _Dept_number = 2_ ocupam dois blocos,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... mas o ponteiro do índice referencia o primeiro desses blocos.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-35.jpg" width="350">

> QUESTÕES:<br>
>> Como lidar com as atualizações de registros e ainda manter o arquivo de dados (e o arquivo de índice) ordenado?<br><br>
>> A estratégia na figura abaixo torna os algoritmos de inclusão e de exclusão de registros mais simples?<br><br>
>> O índice de agrupamento é um índice denso ou um índice esparso? **índice esparso**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-36.jpg" width="400">

<hr style="border:2px solid red">

#### Exercício

**1a. Questão)** Suponha que um arquivo ordenado possui r = 300.000 registros, que estão gravados em um disco com bloco de tamanho B = 4096 bytes. O arquivo está ordenado pelo campo CEP (Código de Endereçamento Postal), e há cerca 1000 CEPs distintos nos registros do arquivo (média de 300 registros por CEP, assumindo distribuição uniforme entre CEPs). Suponha que há um índice de agrupamento:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_(i)_ a chave de ordenação do arquivo de dados tem V = 5 bytes de comprimento, e<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_(ii)_ um ponteiro de bloco tem P = 6 bytes de comprimento.<br>
Determine:<br>
(a) O tamanho **R<sub>i</sub>** de cada entrada (registro) no arquivo de índice de agrupamento.<br>

> R<sub>i</sub> = 5 + 6 = 11 bytes.

(b) O fator de bloco **bfr<sub>i</sub>** do arquivo de índice de agrupamento.<br>

> bfr<sub>i</sub> =  ⎣(B/R<sub>i</sub>)⎦ = ⎣(4096/11)⎦ = 372 entradas de índice por bloco.

(c) O número total de entradas (registros) **r<sub>i</sub>** no arquivo de índice de agrupamento.<br>

> r<sub>i</sub> = 1000 registros (o número de CEPs distintos no arquivo de dados).

(d) O número de blocos **b<sub>i</sub>** do arquivo de índice de agrupamento.<br>

> b<sub>i</sub> = ⎡(r<sub>i</sub>/bfr<sub>i</sub>)⎤ = ⎡(1000/372)⎤ = 3 blocos.

(e) O custo **c<sub>i</sub>** da busca binária no arquivo de índice de agrupamento.<br>

> c<sub>i</sub> =  ⎡(log<sub>2</sub> b<sub>i</sub>)⎤ = ⎡(log<sub>2</sub> 3)⎤ = 2 blocos acessados.

(f) O custo **c<sub>dados</sub>** para localizar os registros de dados, via busca binária no arquivo de índice de agrupamento.<br>

> Visto que há, em média, 300 registros por CEP, os registros com mesmo CEP podem estar dispersos em vários blocos de dados.<br>
No pior caso, c<sub>dados</sub> = c<sub>i</sub> +  ⎡(300 / ⎣(B/R)⎦ )⎤ + 1, onde R é o tamanho do registro de dados.

(g) Quantos bytes ocuparia a estrutura completa de índice de agrupamento? É possível ter essa estrutura em memória?  

> O arquivo de índice de agrupamento possui 3 blocos, então tais blocos ocupam 3 * 4 KBytes, ou seja, 12 KBytes.

[Uma solução](./topico-15solucao-01.md)
