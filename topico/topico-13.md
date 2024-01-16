## [Tópico 13] - Estruturas de indexação (1/5)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

### 1. <ins>VISÃO GERAL</ins>

Considere a presença de um <ins>**ÍNDICE REMISSIVO**</ins>, para a <ins>busca por assunto</ins> em um livro:<br>
&#x267B; O índice apresenta vários assuntos em ordem alfabética,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... apresenta também as páginas do livro (ponteiros) para cada assunto.<br>
&#x267B; O <ins>índice</ins> [remissivo] em geral está nas páginas finais do livro.<br>
&#x267B; Os <ins>dados</ins> estão nas páginas do livro anteriores ao índice [remissivo].<br>
&#x267B; Os <ins>dados</ins> e o <ins>índice</ins> são <ins>estruturas independentes</ins>:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... os <ins>dados</ins> e o <ins>índice</ins> estão em páginas distintas do livro.<br>
&#x267B; VANTAGEM DO ÍNDICE: Rapidez em certas operações sobre os dados:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... agiliza o acesso ao dado.<br>
&#x267B; DESVANTAGEM DO ÍNDICE: Custo [elevado] para atualizar o índice:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... alterações nos dados podem ocasionar modificações no índice;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... exemplos??

No contexto de <ins>arquivos</ins> em sistemas computarizados:
- Índices são <ins>estruturas de acesso auxiliares e adicionais</ins> [aos dados], que são usadas para <ins>acelerar a recuperação de dados</ins>.
- Índices podem ocorrer em arquivos não ordenados, em arquivos sequenciais e em arquivos _hashing_.
- As estruturas de índice são <ins>arquivos adicionais</ins> em memória secundária:
  - fornecem caminhos de <ins>acesso secundário e indireto</ins> aos dados;
  - noutras palavras, os arquivos de índices proveem <ins>maneiras alternativas</ins> de acessar os dados, sem afetar o <ins>posicionamento físico dos registros</ins> no arquivo de dados [em memória secundária].<br><br>

> QUESTÕES:<br>
> 1. **Um arquivo de índice referencia (possui ponteiros para) um arquivo de dados?**<br>
>   - Sim, um arquivo de índice geralmente contém ponteiros para registros no arquivo de dados, facilitando a localização rápida desses registros.
>2. **Um arquivo de índice referencia (possui ponteiros para) um arquivo de dados, e vice-versa?**
>   - Não necessariamente. Embora um arquivo de índice geralmente aponte para um arquivo de dados, a relação não é simétrica. Um arquivo de dados pode não ter um índice correspondente, e vice-versa.
>3. **Dois ou mais arquivos de índice podem referenciar (possuir ponteiros para) um mesmo arquivo de dados?**
>   - Sim, é possível que vários arquivos de índice apontem para o mesmo arquivo de dados, dependendo da estrutura do banco de dados e dos requisitos de consulta.
>4. **Um mesmo arquivo de índice pode referenciar (possuir ponteiros para) dois ou mais arquivos de dados?**
>   - Em geral, um arquivo de índice está associado a um arquivo de dados específico. No entanto, em sistemas mais complexos, pode haver situações em que um índice se relaciona com múltiplos arquivos de dados.
>5. **Um mesmo arquivo de índice pode referenciar (possuir ponteiros para) dois ou mais arquivos de dados, os quais são cópias entre si?**
>   - Sim, em situações em que existem cópias idênticas dos dados em diferentes arquivos, um índice pode apontar para essas cópias.
>6. **Arquivos de índice são estruturados [e alocados] em blocos, similarmente aos arquivos de dados?**
>   - Sim, geralmente os arquivos de índice são estruturados e alocados em blocos, assim como os arquivos de dados, para otimizar operações de leitura e escrita.
>7. **O que significa a sentença 'o acesso aos dados via um arquivo de índice representa um acesso indireto aos dados'?**
>   - Significa que, ao utilizar um arquivo de índice, a localização dos dados não ocorre diretamente, mas através dos ponteiros fornecidos pelo índice. É uma abordagem indireta para acessar os registros desejados.
>8. **O acesso aos dados via o arquivo de índice requer que páginas do arquivo de índice estejam no buffer pool?**
>   - Sim, para otimizar o acesso, as páginas relevantes do arquivo de índice são frequentemente mantidas no buffer pool da memória para reduzir a necessidade de acessos diretos ao disco.

<br>

**CAMPO DE INDEXAÇÃO.**<br>
&#x270D; Campo usado para a construção do índice:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... cada índice possui um campo de indexação;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... no caso do índice remissivo, o campo de indexação é o campo <ins>'assunto'</ins>;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... há outro índice usualmente presente em livros?<br>
&#x270D; Qualquer campo do arquivo pode ser um campo de indexação:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... o campo de indexação pode ser composto por dois ou mais campos.<br>

**REGISTRO NO ARQUIVO DE ÍNDICE.**<br>
&#x270D; O arquivo de índice possui <ins>registros de tamanho fixo</ins>:<br>
&#x270D; Os registros do arquivo de índice segue o padrão: **_< K(i), X >_**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... **_K(i)_** é um valor do campo de indexação, referente ao i-ésimo registro do arquivo;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... **_X_** pode ser:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#10040; o endereço físico de um bloco (ou página) no arquivo de dados;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OU<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#10040; o endereço físico de registro de dados, composto por:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;um endereço físico de bloco; e<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;um ID de registro (ou deslocamento) dentro do bloco;<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OU<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#10040; um endereço lógico do bloco ou do registro dentro do arquivo,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... é um número relativo que seria mapeado para um endereço físico.

**ÍNDICE DENSO _vs._ ÍNDICE ESPARSO.**<br>
&#x270D; Em [arquivos de] <ins>índices densos</ins>, há uma entrada [de índice] para cada valor [do campo de pesquisa] no arquivo de dados (figura abaixo à esq.).<br>
&#x270D; Em <ins>índices esparsos</ins>, há uma entrada [de índice] apenas para alguns dos valores [do campo de pesquisa] no arquivo de dados (figura abaixo à dir.).<br>
&#x270D; Por definição, arquivos de índice denso possuem maior número de registros em relação a arquivos de índice esparso:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x26BE; ... então a busca via um índice esparso é mais rápida do que via um índice denso [para o mesmo arquivo de dados]?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-32.jpg" width="300">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-33.jpg" width="300">
