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
### Rename
```sql
-- Usado para mudar o nome da tabela
alter table cliente
rename to clientes;

