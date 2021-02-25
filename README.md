# Aprenda MySQL

## Descrição
<details>
Sou o tipo de pessoa que gosta de estudar e aliar isso a prática. Não é todo
dia que acordamos com criatividade para criar micro-projetos práticos. Então
decidi unir o útil ao agradável, escrevendo sobre tudo o que eu estou estudando por meio de
repositórios entitulados "learn-alguma-coisa"

Nesse aqui me propolho a escrever sobre MySQL. O meio de aprendizado principal que utilizei foi o [Curso de MySQL](https://www.youtube.com/watch?v=Ofktsne-utM&list=PLHz_AreHm4dkBs-795Dsgvau_ekxg8g1r) do Gustavo Guanabara, no entanto não foi o único.
</details>

## História
<details>
MySQL é um banco de dados relacional gratuito muito popular. Foi criado em 1994 na Suécia,
por Michael Widenius e David Axmark. O MySQl comprou o MySQl em 2007, após a aquisição da Sun pela Oracle, o MySQL ficou sob posse da Oracle, que é uma das maiores produtoras de bancos de dados. Um fork popular do MySQL é o MariaDB.

Algumas empresas que usam o MySQL: Nasa, Wikimedia, Bradesco e etc.
</details>

## Instalação
<details>
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
<details>
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
</details>


## Criando o primeiro banco de dados
<details>
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
</details>

## Melhorando a estrutura do banco de dados
<details>
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
</details>

## Inserindo dados na tabela
<details>
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
</details>

## Alterando estrutura da tabela
<details>
Vamos alterar a estrutura da nossa tabela, adicionando uma nova coluna:

~~~sql
alter table pessoas
add column profissao varchar(30);
~~~

Esse comando adiciona uma coluna chamada _profissao_ na tabela pessoas. Logo todas as linhas da tabela (inclusive as que criamos) terão a coluna profissão. Por padrão, esse comando define a nova coluna como última coluna.

Podemos mudar a posição da coluna caso não quisermos que a mesma seja a última.

Primeiro vamos remover a coluna recém criada:

~~~sql
alter table pessoas
drop column profissao;
~~~

Para inserir uma coluna numa posição específica utilizamos o comando:

~~~sql
alter table pessoas
add column profissao varchar(30) after nome;
~~~

A coluna profissão será adicionada após a coluna nome.
Para adicionar em primeiro lugar, o comando é:

~~~sql
alter table pessoas
add column código int first;
~~~

Sendo assim, utilizamos o _first_ para colocar a nova coluna em primeiro, e o _after_ para coloca-la em outra posição, sendo que por padrão a posição sem especificação ficará como última.

### Alterando o nome da coluna e/ou constraints.

Para alterar o tipo da coluna e suas constraints:

~~~sql
alter table pessoas
modify column profissao varchar(20) not null default '';
~~~

Para alterar o nome, o tipo e suas constraints:

~~~sql
alter table pessoas
change column profissao prof varchar(20);
~~~

Onde _profissao_ é o nome antigo antigo da coluna, e _prof_ o nome atual.

### Alterando o nome da tabela

Alterar o nome de uma tabela é muito simples:

~~~sql
alter table pessoas
rename to grupo;
~~~

### Vamos a prática

~~~sql
-- Cria uma tabela se ela não existir
create table if not exists cursos (
	nome varchar(10) not null unique, -- unique: cada nome deve ser único 
    descricao text,
    carga int unsigned, -- unsigned: numero sem sinal 
    totaulas int unsigned,
    ano year default '2016'
) default charset = utf8;

-- Alterando a tabela para adicionar a coluna idcurso
alter table cursos
add column idcurso int first;

-- Definimos idcurso como chave primária
alter table cursos
add primary key(idcurso);

-- Apagamos a tabela e seus dados (caso ela exista)
drop table if exists cursos;
~~~
</details>

## Manipulando linhas
<details>

* Registro, linha e tupla são sinônimos.
* Linhas são tuplas/registros
* Colunas são campos

Para modificar um campo de um registro, utilizamos o comando update

~~~sql
update cursos set nome='jwt' where idcurso='1'
~~~

para atualizarmos varios campos em uma linha:

~~~sql
update cursos
set nome='html5', descricao='curso de html'
where ano='2015';
limit 1;
~~~

Lẽ-se: atualize a tabela cursos, adicionando os valores 'html5' ao campo 'nome' e 'curso de html' ao 'campo descricao' onde o ano for 2015, porém só retorne o primeiro registro com esse valor.

Para deleter registros

~~~sql
delete from cursos
where idcurso='1';
~~~

Deleter todos registros de uma tabela:

~~~sql
truncate table cursos;
~~~

Também é válido:

~~~sql
truncate cursos;
~~~
</details>

## SELECT
<details>
	
Selecionar todos os registros da tabela cursos:

~~~sql
select * from cursos;
~~~

Selecionar todos os registros ordenados por um campo específico:

~~~sql
select * from cursos
order by nome;
~~~

No sentido ascendente: 

~~~sql
select * from cursos
order by nome asc;
~~~

No sentido descendente:

~~~sql
select * from cursos
order by nome;
~~~

Especificar quais colunas retornar:

~~~sql
select nome, carga, ano from cursos
order by nome;
~~~

Ordernar as colunas por ano, e em seguida por nome:

~~~sql
select nome, carga, ano from cursos
order by ano, nome;
~~~

Filtrar por colunas:

~~~sql
select * from cursos
where ano = '2016';
~~~

Todo resultado de consulta é denominado __result set__.

Podemos utilizar operadores para consultas condicionais com operadores relacionais (>, <, >=, <=)

Nesse caso queremos as colunas nome e ano, onde o ano for maior que 2016:

~~~sql
select nome, ano from cursos
where ano > '2016';
~~~

Maior e menor:

~~~sql
select nome, ano from cursos
where ano <> '2016';
~~~

Definindo um intervalo específico:

~~~sql
select nome, ano from cursos
where ano between 2018 and 2019
order by nome desc;
~~~

Definindo valores específicos para a consulta:

~~~sql
select nome, ano from cursos
where ano in (2014, 2016, 2017) -- conjunto x
~~~

Onde serão retornados os registros, _se e somente se_ o ano pertencer ao conjunto x

Veja alguns exemplos com operadores lógicos:

~~~sql
select * from cursos
where carga > 20 and totaulas > 32;

select * from cursos
where carga > 20 or totaulas > 32;
~~~

Utilizamos o operador `like` para obter resultados semelhantes, e utilizamos o caractere coringa `%` que serve para substituir nenhuma ou várias ocorrências.

Abaixo retornamos todo registro cujo nome tiver como inicial a letra `J`:

~~~sql
select * from cursos
where nome like 'J%';
~~~

O operador 'not` representa a negação:

~~~sql
select * from cursos
where nome not like '%J%';
~~~

O exemplo acima seleciona qualquer registro que não contenha a letra `J` no `nome`.

Ao invés do `%` você pode utilizar o `_`, que significa que obrigatóriamente deve ter um caracter ali, seja numérico ou não.

Tanto `%` como `_` são considerados `wildcards `.

Por sua vez o `distinct` faz distinção. Reduzindo todas ocorrência repetidas a somente uma:

~~~sql
select distinct [coluna] * from [nome_da_tabela]
~~~

Podemos contar registros utilizando a função de agregação `count()`

~~~sql
select count(*) from tabela; -- conta todos os registros da tabela
~~~

Algumas funções de agregação:

* `sum()`: soma 
* `avg()`: média
* `max()`: máximo
* `min()`: minimo


Vamos a função de agrupamento:

~~~sql
select totaulas, count(*) from cursos;
group by totaulas;
~~~

Isso agrupa campos. No caso acima serão criados grupos para cada total de aulas.

O `having` só pode ser utilizado quando o campo for o mesmo que o `group by`:

~~~sql
select ano, count(*) from cursos
group by ano 
having count(ano) >= 5
order by count(*) desc;
~~~ 

</details>

## Chaves estrangeiras e JOIN

Toda transação deve seguir o principio ACID

* Atomicidade: ou toda tarefa é realizada, ou nada é considerado.
* Consistência: se antes de uma transação o banco dados está "ok", então após ela deve permancer "ok", mantendo assim a consistência do todo.
* Isolamento: quando temos duas transações acontecendo em paralelo, elas devem ocorrer de modo isolado, ou seja, uma não poder intervir na outra.
* Durabilidade: Os dados devem persistir o enquanto forem necessários.

### Cardinalidade e alguns tipos de relacionamentos

São eles: 1:N, N:N e 1:1.

Por exemplo, considere um banco de dados desenhado para manter informações relativas a um hospital. Esse banco de dados poderá ter várias tabelas como:

Tabela médico onde constará informações sobre o médico profissional;
Tabela paciente onde constará dados relativos aos assuntos médico e sobre o tratamento do paciente;
Tabela departamento onde será tratado as informações relativas as divisões departamentais do hospital.

Neste modelo teremos o seguinte cenário:

Existirá o relacionamento vários-para-vários (N:N) entre os registros da tabela médico e os registro da tabela paciente, um médico atende diversos pacientes, assim como um paciente pode ser atendido por diversos médicos;

Existirá o relacionamento um-para-vários (1:N) no relacionamento entre a tabela departamento em relação a tabela de médicos, pois um médico, poderá trabalhar em somente um departamento do hospital, contudo, um departamento poderá ter vários médicos.

Já o relacionamento um-para-um (1:1) será usado nos casos onde o registro de uma tabela só poderá ter uma associação com um registro de outra tabela. No nosso caso, isso caberia na relação entre um quarto de apartamento e um paciente. Pois um paciente só poderá estar em um determinado apartamento, e cada apartamento só poderá abrigar um determinado paciente (partindo do princípio de quartos individuais).

#### Um-para-muitos

No relacionamento 1:N a tabela que receberá a chave estrangeira será a tabela _N_

### Criando chave estrangeira na prática


Para criarmos nossa chave estrageira o comando abaixo é necessário:

~~~sql
-- Adicionando a coluna que receberá a chave estrangeira
alter table gafonhotos add column cursopreferido int;

-- Definindo chave estrangeira
alter table gafanhotos
add foreigh key (cursopreferido) 
references cursos (idcursos);

-- primeira referência:
update gafanhotos set cursopreferido = '6' where id = '1';
~~~

## Join

Podemos visualizar os dados de uma tabela com o Join

~~~sql
select gafanhotos.nome, cursos.nome, cursos.ano
from gafanhotos inner join cursos
on cursos.idcurso = gafanhoto.cursopreferido
~~~

Seleciona o nome do aluno, o nome do curso, o ano do cursos das tabelas gafanhoto e cursos
juntando as colunas selecionadas numa única tabela.
Onde o id do curso for igual ao curso preferido.
O `on`  sempre será utiizando para termos referencia dos dados.

O `join` puro é considerado um _inner join_ ou seja, ele faz a junção baseado nas relações.

Dá para simplificar criando alias dos nomes das tabelas:


~~~sql
select g.nome, c.nome, c.ano
from gafanhotos as g inner join cursos as c
on c.idcurso = g.cursopreferido
~~~

Para mostrarmos todos as colunas independente de relacionamento, utilizamos o comando `outer join` 

~~~sql
select g.nome, c.nome, c.ano
from gafanhotos as g outer join cursos as c
on c.idcurso = g.cursopreferido
~~~
Podemos priorizar os dados da tabela da esquerda ou direita com _left join_ e _right join_

Esquerda:

~~~sql
select g.nome, c.nome, c.ano
from gafanhotos as g right outer join cursos as c
on c.idcurso = g.cursopreferido
~~~

Direita:

~~~sql
select g.nome, c.nome, c.ano
from gafanhotos as g left outer join cursos as c
on c.idcurso = g.cursopreferido
~~~

## Inner Join com várias tabelas

~~~sql
-- Tabela intermediária para resolver uma relação N:N
create table gafanhoto_assiste_cursos (
  id int not null  auto_increment,
  data date,
  idgafanhoto int,
  id curso int,
  primary key (id),
  foreigh key (idgafanhoto) references gafanhoto(id),
  foreigh key (idcurso) references cursos (idcurso)

) default charset = utf8;

insert into gafanhoto_assiste_cursos values
(default, '2010-05-10', '1', '2');


select g.nome, c.nome from gafanhotos g
join gafanhoto_assiste_cursos a
on g.id = a.idgafanhoto 
join curso c 
on c.cursoid = a.idgafanhoto
~~~
