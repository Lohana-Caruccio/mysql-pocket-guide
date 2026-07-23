 <h1 align="center">🐬MySQL-Pocket-Guide / Guia de bolso MySQL🐬</h1>
 
### Esse repositório contém anotações e lembretes dos meus estudos em MySQL. Pensado principalmente, para consultas rápidas do dia a dia.

### Comandos básicos com exemplos:
#### Criar banco de dados
```sql
create database nomedoBD;
```
#### Aponta para qual banco de dados se quer usar
```sql
use database;
```
#### Confere qual banco de dados está selecionado
```sql
select database();
```
#### Mostra as tabelas já criadas
```sql
show tables;
```
#### Apaga a tabela escolhida, ou qualquer outro elemento
```sql
drop table nometabela;
drop database nomedoBD;
```
#### Apaga os dados(linhas) de uma tabela
```sql
delete from table
where id = 5;

delete * from cliente;
```
#### Apaga todos os dados de uma tabela de uma vez só
```sql
truncate table cliente;
```
#### Criação de tabelas
```sql
create table cliente (
id int auto_increment,
nome varchar(100) not null,
genero char(01) check (genero in('F', 'M', 'O')),
nascimento date,
email varchar(120) unique,
cidadeId int,

constraint pk_idcliente primary key(id),
constraint fk_cidadecliente foreign key (cidadeId) references cidade(id)

);
```
#### Inserção de dados nas tabelas
```sql
-- Forma completa
insert into cliente (nome, genero, nascimento, email, cidadeId) values ('Julia', 'F', '2003-04-25','julia@gmail.com', 2);
-- Forma reduzida
insert into cliente values ('Julia', 'F', '2003-04-25','julia@gmail.com', 2);
```
#### Conferir a estrutura de uma tabela existente
```sql
-- Traz os tipos de dados e as propriedades
describe cliente;
```

### Comandos de alteração(ALTER) com exemplos:
#### Add
```sql
alter table cliente
add telefone varchar(15);
-- Também pode-se adicionar uma pk ou fk com ele
alter table alunos
add constraint pkAluno primary key(id);
```
#### Change
```sql
-- Usado mais para mudar o nome da coluna mas pode-se mudar o tipo de dado também
alter table cliente
change telefone celular varchar(14);
```
#### Modify
```sql
-- Usado mais para apenas mudar as propriedades da coluna
alter table cliente
modify celular varchar(15) not null;
```
#### Rename
```sql
-- Usado para mudar o nome da tabela
alter table cliente
rename to clientes;
```


### Comando de atualização:
#### Update
```sql
--- Usado para atualizar registros existentes
update cidade
set nome = 'Gramado'
where id = 2;
--- nesse exemplo iria atualizar na tabela cidade onde o id é 2, cujo nome da cidade era Bagé, para Gramado
```

### Exclusão e Restrição de consultas
#### Select com where (exemplos)
``` sql
--- ex1
select from cliente
where cidadeid = 1
and salario > 8000;

--- ex2, mostrará todas as colunas
select from cliente
where cidadeid = 1
or cidadeid = 3
or cidadeid = 5;

--- ex3, mostrará somente as colunas pedidas
select nome, salario from cliente
where cidadeid = 2;
--- forma reduzida
select nome, salario from cliente
where cidadeid in (1,3,5);

--- ex5
select nome, salario from cliente
where salario >= 5000
and salario <= 10000;
--- forma alternativa
select nome, salario from cliente
where salario between 5000 and 10000;
```
#### Select com order by (deve ser sempre a última cláusula do select)
```sql
--- ex1
select nome, salario from cliente
order by nome asc/desc;

--- ex2
select nome, salario from cliente
where salario between 5000 and 10000
order by salario;
--- forma alternativa
select nome, salario from cliente
where salario between 5000 and 10000
order by 2(segunda coluna = salario);
```

#### Comando JOIN
##### Inner JOIN (junta informações comuns de duas tabelas)
``` sql
--- ex1
select nomeCidade, nomeEstado
from cidade
inner join estado
on cidade.Estadoid = estado.id;
--- forma alternativa mais antiga
select nomeCidade, nomeEstado
from cidade, estado
where cidade.Estadoid = estado.id;
```

##### Left JOIN (indica que quer que apareça todas as linhas da tabela à esquerda)
``` sql
--- nesse caso a tabela à esquerda é cidade
select nomeCidade, nomeEstado
from cidade
left join estado
on cidade.Estadoid = estado.id;
```

##### Right JOIN (indica que quer que apareça todas as linhas da tabela à direita)
```sql
--- nesse caso a tabela à direita é estado
select nomeCidade, nomeEstado
from cidade
right join estado
on cidade.Estadoid = estado.id;
```
##### Cross JOIN (serve para gerar todas as combinações possíveis entre duas listas)
```sql
select nomeCidade, nomeEstado
from cidade, estado
order by nomeCidade;
```

##### Self JOIN (consulta da tabela com ela mesma)
```sql
--- nesse exemplo, seria uma consulta na tabela funcionario onde funcionarios também possuem gerentes, mas os gerentes também são funcionários
select funcionario.nomeFun, gerente.nomeFunc
from funcionario
inner join funcionario gerente
on funcionario.gerente = gerente.matricula;
```

#### ALIAS (apelido)
##### Exemplo
```sql
--- na hora de aparecer a tabela mudará o nome somente nesse momento
select nomeFunc as 'Nome do funcionario' from funcionario f
where f.cidadeid = 1;
```

### Limit
##### Exemplo 
```sql
--- traz somente as 3 primeiras linhas
select * from funcionario limit 3;
```

#### UNION
##### Exemplo union
```sql
--- traz em um resultado só os nomes dos funcionarios e os nomes das cidades, porém se houver uma pessoa que tenha o mesmo nome de uma cidade por exemplo, ele trará uma vez só
select nomeFunc from funcionario
union
select nomeCidade from cidade
```

##### Exemplo union all
```sql
--- traz em  um resultado só os nomes dos funcionarios e os nomes das cidades, todos mesmo repetidos
select nomeFunc from funcionario
union all
select nomeCidade from cidade
```
#### Comando Case (condição)
##### Exemplo
```sql
---
select nomeFunc ' Nome do funcionario',
case
    when sexoFunc = 'F' then 'Feminino'
    when sexoFunc = 'M' then 'Masculino'
end as 'Genero'
from funcionario;
```
