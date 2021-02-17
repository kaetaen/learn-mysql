# Aprenda MySQL

## Descrição

Sou o tipo de pessoa que gosta de estudar e aliar isso a prática. Não é todo
dia que acordamos com criatividade para criar micro-projetos práticos. Então
decidi unir o útil ao agradável, escrevendo sobre tudo o que eu estou estudando por meio de
repositórios entitulados "learn-alguma-coisa"

Nesse aqui me propolho a escrever sobre MySQL. O meio de aprendizado principal que utilizei foi o [Curso de MySQL](https://www.youtube.com/watch?v=Ofktsne-utM&list=PLHz_AreHm4dkBs-795Dsgvau_ekxg8g1r) do Gustavo Guanabara, no entanto não foi o único.


## Indice

1. [História](#history)
2. [Instalação](#install)
3. [Classificação de comandos]("#sqlclass")
4. [Criando primeiro banco de dados]("#firstdatabase")



## História <a name="history"></a>
MySQL é um banco de dados relacional gratuito muito popular. Foi criado em 1994 na Suécia,
por Michael Widenius e David Axmark. O MySQl comprou o MySQl em 2007, após a aquisição da Sun pela Oracle, o MySQL ficou sob posse da Oracle, que é uma das maiores produtoras de bancos de dados. Um fork popular do MySQL é o MariaDB.

Algumas empresas que usam o MySQL: Nasa, Wikimedia, Bradesco e etc.

## Instalação <a name="install"></a>

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

## Classificação de comandos <a name="sqlclass"></a>

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

## Criando o primeiro banco de dados <a name="firstdatabase"></a>

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


