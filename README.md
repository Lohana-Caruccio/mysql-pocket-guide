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

#### Limit
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

### SUBCONSULTAS/ SUBQUERYS
#### Subconsulta simples
```sql
--- Faz primeiro a consulta interna e depois a externa, mostrando todas as colunas do cliente específico onde for achado o cidadeId == 'Curitiba'
select * from cliente
where cidadeId = (select id from cidade where nome = 'Curitiba');
```

#### Subconsulta com dois parâmetros
```sql
select * from cliente
where cidadeId in (select id from cidade 
					where nome = 'Passo de torres' or nome = 'Curitiba');
```

#### Comando EXISTS (retorna caso sim ou caso não)
```sql
--- Nesse caso, se houver clientes ganhando mais que 2700, retornará o nome e salário dos que ganham menos que 2800, pode-se colocar not exists para negar
select nome, salario
from cliente
where salario < 2800
and exists(select * from cliente where salario > 2700);
```

#### Comando ANY
```sql
--- é equivalente a "pelo menos um"
select * from cliente
where id > any(select distinct clienteid from vendas);
```

#### Comando ALL
```sql
--- é equivalente a "todos eles"
select * from cliente
where id > all(select distinct clienteid from vendas);
```

#### Comando INTERSECT (mesma função do inner join)
```sql
--- Nesse caso, só mostra quando o id da cidade existe na tabela cidadeId de um cliente
select cidadeId from cliente
intersect
select id from cidade;
```

### FUNÇÕES DE FORMATAÇÃO
#### Dados textuais
##### Length
```sql
--- Mostra o tamanho de caracteres, nesse caso do nome e dataNascimento
select nome, length(nome), length(dataNascimento) from cliente;
```

##### Upper e Lower
```sql
--- Joga tudo para maiúsculo ou minúsculo
select upper(nome), lower(nome) from cliente;
```

##### Trim, Ltrim, Rtrim
```sql
--- Remove os espaços em branco
--- exemplo com ltrim e rtrim
select ltrim(nome), rtrim(nome) from cliente;

--- exemplo de remover ambos ao mesmo tempo
select trim(both from nome), nome from cliente;
```

##### Substring
```sql
--- Pega somente a quantidade que você pedir de caracteres, nesse caso do nome vai pegar a partir do quinto carctere em diante
select substring(nome, 5), nome from cliente;
```

##### Cast
```sql
--- Muda o tipo de dado
select cast('2020-12-30' as date), cast('1000.99' as float);
```
#### Dados numéricos e temporais
##### - NUMÉRICO
###### Round e Truncate
```sql
--- Round arredonda o valor numérico (com opção de casas decimais)
select salario, round(salario, 2) from cliente;

--- Truncate corta o número na quantidade de casas decimais sem arredondar
select salario, truncate(salario, 2) from cliente;
```

###### Mod e Div
```sql
--- Mod retorna o resto de uma divisão
select mod(id, 2) from cliente;

--- Div realiza a divisão inteira (descarta as casas decimais)
select id div 2 from cliente;
```

##### - DATA E TEMPORAIS

###### Curdate, Curtime e Now
```sql
--- Retornam a data atual, hora atual ou ambos juntos
select curdate(), curtime(), now();
```

###### Date, Month, Monthname, Year, Day, Week e Weekday
```sql
--- Extraem partes específicas de uma coluna do tipo Data
select 
    dataNascimento,
    date(dataNascimento),
    month(dataNascimento),
    monthname(dataNascimento),
    year(dataNascimento),
    day(dataNascimento),
    week(dataNascimento),
    weekday(dataNascimento) 
from cliente;
```

###### Adddate e Datediff
```sql
--- Adddate adiciona um intervalo de dias ou tempo a uma data
select adddate(dataNascimento, interval 30 day) from cliente;

--- Datediff calcula a diferença em dias entre duas datas
select datediff(now(), dataNascimento) from cliente;
```

###### Date_format
```sql
--- Formata a data para o padrão desejado (ex: dia/mês/ano)
select date_format(dataNascimento, '%d/%m/%Y') from cliente;
```

###### Time, Timediff e Addtime
```sql
--- Time extrai a parte da hora de um campo datetime
select time(now());

--- Timediff calcula a diferença entre dois horários
select timediff('18:00:00', '08:00:00');

--- Addtime adiciona um tempo/intervalo a uma hora especificada
select addtime(curtime(), '02:00:00');
```

###### Timestamp, Timestampadd e Time_format
```sql
--- Timestamp converte ou combina uma expressão em data e hora completas
select timestamp(dataNascimento) from cliente;

--- Timestampadd adiciona um intervalo específico (MONTH, DAY, HOUR, etc.) a uma data
select timestampadd(month, 6, dataNascimento) from cliente;

--- Time_format formata um valor de hora (horas, minutos, segundos)
select time_format(curtime(), '%H:%i') as hora_minuto;
```


### AGREGACÃO E EXTRAÇÃO DE DADOS
##### Count e Sum
```sql
--- Count conta o número total de registros/linhas
select count(*) from cliente;

--- Sum soma todos os valores de uma coluna numérica
select sum(salario) from cliente;
```

##### Min, Max e Avg
```sql
--- Min e Max retornam o menor e o maior valor de uma coluna
select min(salario), max(salario) from cliente;

--- Avg calcula a média aritmética dos valores de uma coluna
select avg(salario) from cliente;
```

##### Group By
```sql
--- Agrupa os registros com base em uma ou mais colunas para realizar cálculos por grupo
--- Nesse exemplo somará se baseando por cada cidade, a soma dos salários
select cidadeId, sum(salario) from cliente
group by cidadeId;
```

##### Having
```sql
--- Nesse caso, faz quase a mesma coisa que o exemplo anterior, mas só mostrará a soma dos que forem maior que 3000
select cidadeId, sum(salario) from cliente
group by cidadeId
having sum(salario) > 3000;
```

### INTEGRIDADE E SEGURANÇA DE DADOS

#### Create User
```sql
--- Cria um novo usuário no sistema com uma senha definida
create user 'dev_usuario'@'localhost' identified by 'senha123';
```

#### Grant
```sql
--- Concede permissões específicas (ex: SELECT, INSERT) para o usuário na tabela cliente
grant select, insert on aula.cliente to 'dev_usuario'@'localhost';

--- Concede todas as permissões em todas as tabelas do banco de dados aula
grant all privileges on aula.* to 'dev_usuario'@'localhost';
```

#### Revoke 
```sql
--- Remove permissões específicas previamente concedidas ao usuário
revoke insert on aula.cliente from 'dev_usuario'@'localhost';

--- Remove todos os privilégios do usuário no banco de dados
revoke all privileges, grant option from 'dev_usuario'@'localhost';
```

#### Flush Privileges
```sql
--- Recarrega as tabelas de permissão do MySQL para garantir que as alterações entrem em vigor imediatamente
flush privileges;
```

### Outros comandos
#### Index
```sql
--- Cria um índice para acelerar consultas e buscas pela coluna nome, nesse exemplo pessoa é uma tabela criada com nome, email e data de nascimento
create index idxPessoa on pessoa(nome);

--- Remove o índice criado previamente caso ele não seja mais necessário
drop index idxPessoa on pessoa;
```

#### View
```sql
--- Cria uma tabela virtual (View) para simplificar consultas frequentes aos dados de contato
create view mostraPessoa
as 
	select nome as 'Nome de pessoa',
    email from pessoa;

--- Consulta os dados diretamente pela View como se fosse uma tabela comum
select * from mostraPessoa;

--- Exclui a View criada do banco de dados
drop view mostraPessoa;
```

#### Transactions
```sql
--- Inicia um bloco de transação segura para garantir que todas as operações ocorram juntas
start transaction;

--- Executa as alterações necessárias na tabela pessoa
insert into pessoa (nome, email, dataNascimento) values('Zanana', 'zanana@gmail.com','2000-09-15');
select * from pessoa;

--- Confirma e salva permanentemente todas as alterações feitas na transação
commit;

--- (Opção de emergência) Desfaz todas as alterações caso algo dê errado antes do commit
rollback;

--- Lembrando que para as transactions funcionarem deve-se usar esse comando
set autocommit = off;
```

### Trigger
```sql
--- Cria uma tabela de auditoria para registrar históricos de alterações
create table auditoria(
	id_aud int auto_increment primary key,
	acao varchar(50),
    id_func int,
    salario decimal (10,2),
    novoSalario decimal(10,2),
    dataOperacao date
);

--- Cria um gatilho para salvar automaticamente o histórico sempre que o salário for alterado
delimiter $$
create trigger alterafuncionario after update
on funcionario
for each row
begin
	insert into auditoria (acao, id_func, salario, novoSalario, dataOperacao)
	values ('alteração', new.id_func, old.salario, new.salario, curdate());
end$$
delimiter ;

--- Lista todas as triggers criadas na base de dados atual
show triggers;

--- Remove a trigger do banco de dados quando ela não for mais necessária
drop trigger alterafuncionario;
```
