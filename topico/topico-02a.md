## [Tópico 02a] - Exercícios de revisão (1/3)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

<hr style="border:2px solid red">


#### <ins>EXERCÍCIO:</ins> Um banco de dados simples (**BD Simples**):

> <ins>Esquema conceitual</ins>:

<img src="../media/fig-der-simples-1.jpg" width="350">

> <ins>Esquema lógico</ins>:

&#8718; PRODUTO(<ins>CodProduto</ins>, Descrição, Preço, _Categ_)<br>
&#8718; CATEGORIA(<ins>CodCateg</ins>, Nome)<br>
&#8718; PRODUTO(Categ) REFERENCIA CATEGORIA(CodCateg)<br>

> <ins>Ilustração</ins>:

CATEGORIA
| <ins>CodCateg</ins> | Nome |
|-|-|
| **papel** | Papelaria e escritório |
| **esporte** | Material esportivo |

PRODUTO
| <ins>CodProduto</ins> | Descrição | Preço | _Categ_ |
|-|-|-|-|
| **1234** | Lápis | 0.70 | _papel_ |
| **1111** | Bola | 20.00 | _esporte_ |
| **2222** | Caneta | 1.20  | _papel_ |
| **1212** | Meião | 12.40 | _esporte_ |
| **1112** | Viseira| 12.40 | _esporte_ |

Quais as restrições de integridade - _domínio_, _chave_, _integridade de entidade_ e _integridade referencial_ - são violadas em cada uma das seguintes operações?<br>
**(a)** Incluir a _tupla_ **<NULL,"Boné",12.00,"vestuário">** em PRODUTO.<br>
>  (2) e (8)

**(b)** Incluir a _tupla_ **<1212,"Borracha",2.10,"papel">** em PRODUTO.<br>
> 2

**(c)** Incluir a _tupla_ **<1212,2.10,"Borracha","papel">** em PRODUTO.<br>
> 1, 2,4 

**(d)** Alterar a _tupla_ **<"papel","Papelaria e escritório">** para **<"papeis","Papelaria e Outros">** em CATEGORIA.<br>
>

**(e)** Excluir a _tupla_ **<"papel","Papelaria e escritório">** em CATEGORIA.<br>
> 8

**(g)** Alterar a _tupla_ **<"papel","Papelaria e escritório">** para **<"papelaria","Papelaria e Escritório">** em CATEGORIA.<br>
>

**(h)** Excluir a _tupla_ **<1111,"Bola",20.00,"esporte">** em PRODUTO.
>


IMPORTANTE:<br>
&#8718; Para analisar cada operação, considere o banco de original conforme a ilustração. Por exemplo, para analisar a operação em (d), desconsidere possíveis modificações no banco de dados pelas operações em (a), (b) e (c).<br>
&#8718; Ao responder, calcule o somatório dos números que identificam cada restrição violada pela operação:<br>
(01) Restrição de domínio<br>
(02) Restrição de chave<br>
(04) Restrição de integridade de entidade<br>
(08) Restrição de integridade referencial<br>

> - **(01) Restrição de Domínio:** Garante que os valores em uma coluna estejam corretos de acordo com o tipo de dado e faixa de valores permitidos.

> - **(02) Restrição de Chave:** Garante que valores em uma ou mais colunas identifiquem exclusivamente cada linha em uma tabela, evitando duplicatas.

> - **(04) Restrição de Integridade de Entidade:** Garante que cada linha em uma tabela seja única, impedindo duplicatas de linhas.

> - **(08) Restrição de Integridade Referencial:** Mantém a consistência nas relações entre tabelas, garantindo que os valores em uma coluna que faz referência a outra tabela correspondam a valores válidos na tabela referenciada.


<hr style="border:2px solid red">
