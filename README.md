# Aprenda MySQL

## Descrição

Sou o tipo de pessoa que gosta de estudar e aliar isso a prática. Não é todo
dia que acordamos com criatividade para criar micro-projetos práticos. Então
decidi unir o útil ao agradável, escrevendo sobre tudo o que eu estou estudando por meio de
repositórios entitulados "learn-alguma-coisa"

Nesse aqui me propolho a escrever sobre MySQL. O meio de aprendizado principal que utilizei foi o [Curso de MySQL](https://www.youtube.com/watch?v=Ofktsne-utM&list=PLHz_AreHm4dkBs-795Dsgvau_ekxg8g1r) do Gustavo Guanabara, no entanto não foi o único.

## História
<details>
MySQL é um banco de dados relacional gratuito muito popular. Foi criado em 1994 na Suécia,
por Michael Widenius e David Axmark. O MySQl comprou o MySQl em 2007, após a aquisição da Sun pela Oracle, o MySQL ficou sob posse da Oracle, que é uma das maiores produtoras de bancos de dados. Um fork popular do MySQL é o MariaDB.

Algumas empresas que usam o MySQL: Nasa, Wikimedia, Bradesco e etc.
</details>

## Instalação
</details>
Primeiro vamos atualizar o indíce dos nossos pacotes.
~~~bash
$ sudo apt update
~~~
A seguir instala o pacote:
~~~bash
$ sudo apt install mysql-server
~~~
Execute o MySQL:
~~~bash
sudo mysql
~~~
Configure e defina a senha para o banco de dados:

~~~bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
~~~
Por fim recarregue as tabelas de permissões e coloque as alterações em vigor.

~~~bash
mysql> FLUSH PRIVILEGES;
~~~

Para acessar o mysql digite o comando abaixo, pressione enter e insira a senha que criamos
~~~bash
$ mysql -u root -ṕ
~~~
</details>
## Classificação de comandos

A linguagem SQL é uma só, contudo a mesma pode ser subdividida e classificada  nos seguintes grupos (tipos) de comandos denominados como linguagens

Esses "tipos" são:

* **DDL**
 
  Data Definition Language (Linguagem de Definição de Dados) é responsável pela interação com objetos do banco.
  São comandos DDL: CREATE, ALTER, DROP

* **DML**
  
  Data Manipulation Language (Linguagem de Manipulação de Dados) é responsável pela manipulação de dados dentro das tabelas.
  São comandos DML: INSERT, DELETE, UPDATE

* **DQL**

  Data Query Language (Linguagem de Consulta de Dados) responsável pela consulta de dados.
  O comando DQL __SELECT__   é o comando de consulta, contudo em algumas fontes ele é incluso dentro do DML.

* **DTL**

  Data Transaction Language (Linguagem de Transação de Dados) são comandos para controle de transação
  São comandos DTL: BEGIN DATA TRANSACTION, COMMIT, ROLLBACK.
  Características de transações: D.I.C.A. (Durabilidade, Isolamento, Consistência, Atomicidade) deve ser seguido

* **DCL**

  Data Control Language (Linguagem de Controle de Dados) define o controle de acesso ao banco.

## Criando o primeiro banco de dados

Para criar um banco de dados é necessário rodar o comando:

~~~sql
mysql> create database cadastro;
~~~

"cadastro" é o nome do nosso banco de dados.

Agora acesse o banco de dados que você criou
~~~sql
use cadastro;
~~~

Antes de criar nossa tabela, vamos dar uma olhada nos tipos primitivos do MySQL

### Tipos primitivos

| GRUPO 	| TIPO | PRECISÃO |
| --- 		| ---  | ---	  |
| Numérico 	| 1. Inteiro<br> 2. Real <br> 3. Lógico | 1.  TinyInt, SmallInt, Int, MediumInt, BigInt <br> 2. Decimal, Float, Double, Real <br> 3. Bit, Boolean|
| Data/Tempo 	| ... | 0. Date, DateTime, TimeStamp, Time, Year |
| Literal	| 1. Caractere <br> 2. Texto <br> 3. Binário <br> 4. Coleção  | 1. Char, VarChar <br> 2. TinyText, Text, MediumText, LongText <br> 3. TinyBlob, Blob, MediumBlob, LongBlog <br> 4. Enum, Set|
| Espacial	| ... | 0. Geometry, Point, Polygon, MultiPolygon |

### Comandos de navegação e descrição 

Esses são alguns comandos de navegação e descrição que são importantes dentro do mysql

~~~sql
show databases;  -- mostra os bancos de dados criados

use nome_do_banco; -- acessa o banco de dados atual

status;  -- verifica qual banco de dados está aberto

show tables; -- mostra quais são as tabelas dentro do banco atual

describe nome_da_tabela; -- mostra um resumo da tabela 
~~~

Vamos criar nossas colunas dentro da tabela `cadastro`

~~~sql
create table pessoas (
    nome varchar(30),
    idade tinyint,
    sexo char(3),
    peso float,
    altura float,
    nacionalidade varchar(20)
);
~~~

Criamos uma tabela chamada `pessoas` dentro do banco de dados `cadastro`. Na tabelas `pessoas` temos as colunas seguidas de seus respectivos tipos.

## Melhorando a estrutura do banco de dados

Vamos refatorar a forma como criamos e formulamos nossos dados e tabelas. Antes disso vamos remover nosso banco de dados para recriarmos ele do "jeito certo".

~~~sql
drop database cadastro;
~~~

Vamos criar nosso banco de dados

~~~sql
create database cadastro
default character set utf8
default collate utf8_general_ci;
~~~

Com isso utilizamos as **constraints** e **collations** necessárias para que nosso banco de dados tenha um bom suporte para o UTF-8.

**Constraints** (restrições) mantém os dados do usuário restritos, e assim evitam que dados inválidos sejam inseridos no banco, collations por sua vez, definem o conjunto de regras que o servidor irá utilizar para ordenação e comparação entre textos, ou seja, como será o funcionamento dos operadores =, >, <, order by, etc.

Então, criamos nossa tabela com suas respectivas colunas:

~~~sql
create table pessoas (
    id int not null auto_increment,
    nome varchar (30) not null,
    nascimento date,
    sexo enum('M', 'F'),
    peso decimal(5, 2),
    altura decimal (3, 2),
    nacionalidade varchar(20) default 'Brasil',
    primary key (id)
) default charset = utf8;
~~~

Criamos nossa tabela mais consistente, vamos entender o funcionamento de alguns comandos e constraints:

* not null: valor obrigatório.
* auto_increment: o valor será incrementado automaticamente;
* primary key: define a chave primária.
* default; define valor padrão.

Sobre alguns tipos:

* enum('M', 'F'): o campo aceitará somente os valores 'M' ou 'F' (case sensitive).
* decimal(n, 2): o valor será do tipo decimal, ocupando _n_ digitos com dupla precisão. Exemplo: decimal(5, 2) == 321,56

## Inserindo dados na tabela

Inserir dados em uma tabela é simples:

~~~sql
insert into pessoas (id, nome, nascimento, sexo, peso, altura, nacionalidade)
values (default, 'Paul', '1984-01-02', 'M', '78.5', '1.83', 'Japão');
~~~

Se os valores estiverem na ordem das colunas da tabela, então podemos simplificar a sintaxe:
~~~sql
insert into pessoas values
(default, 'Lia', '1964-10-12', 'F', '108.5', '1.60', 'Italia');
~~~

Em caso queira inserir múltiplas linhas em um único comando:

~~~sql
insert into pessoas (id, nome, nascimento, sexo, peso, altura, nacionalidade)
values
(default, 'Jean', '1999-01-02', 'M', '60.9', '1.93', 'França'),
(default, 'Max', '1996-12-25', 'M', '78.5', '1.83', 'Inglaterra'),
(default, 'Helena', '1985-01-01', 'F', '70.1', '1.60', default);
~~~

A forma simplificada também é valida aqui:

~~~sql
insert into pessoas
values
(default, 'Jean', '1999-01-01', 'M', '60.9', '1.93', 'França'),
(default, 'Max', '1996-12-25', 'M', '78.5', '1.83', 'Inglaterra'),
(default, 'Helena', '1985', 'F', '70.1', '1.60', default);
~~~

Se quiser conferir se os valores foram inseridos, recupere todos os dados com:

~~~sql
select * from pessoas
~~~

