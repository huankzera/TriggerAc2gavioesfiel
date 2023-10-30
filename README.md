# TriggerAc2gavioesfiel
# Trigger.
## A atividade a seguir possui o objetivo de realizar a utilização do TRIGGER, que basicamente consiste em ser um ferramenta, armazenado no bancos de dados que é chamado automaticamente sempre que ocorre um evento especial no banco de dados, um acionador pode ser chamado quando uma linha é inserida em uma tabela especificada ou quando determinadas colunas da tabela estão sendo atualizadas.

## 1º REPRODUZA O PRIMEIRO CÓDIGO SUGERIDO NO MYSQL WORKBENCH; 
```SQL
Inserir aqui
create table Pedido(
IDpedido int auto_increment primary key,
DataPedido datetime,
NomeCliente varchar(100)
);

insert into Pedido (DataPedido, NomeCliente) values ('2005-09-18', 'Matheus');
insert into Pedido (DataPedido, NomeCliente) values ('2002-03-13', 'Julio');
insert into Pedido (DataPedido, NomeCliente) values ('2007-02-13', 'Henrique');
```
![image](https://github.com/huankzera/TriggerAc2gavioesfiel/assets/126423433/1fceb437-ffb4-4909-a83f-2da2cd794b3d)


## 2º EXECUTE AS ETAPAS E VERIFIQUE SEUS RESULTADOS; 
```SQL
Inserir aqui
DELIMITER $
create trigger RegistroDataCriacaoPedido
before insert on Pedido
for each row 
begin 

set New.DataPedido = now();
end;
$
DELIMITER ;

insert into Pedido (NomeCliente) values ('Maria');

select * FROM PEDIDO; 
```


## Observação: A utilização do trigger para a tabela citada no exemplo, serve para acrecentar mais um Cliente, sem alteração na sua tabela inicial, fazendo com que coloque apensas o nome como informação do cliente. 


## APÓS A EXECUÇÃO DO PRIMEIRO CÓDIGO REALIZE O SEGUNDO EXEMPLO;
FAÇA AS ETAPAS INDICADAS DO SEGUNDO EXEMPLO;

```SQL
inserir aqui
create table Filmes(
id int  primary key auto_increment,
titulo varchar(100),
minutos int 
); 

delimiter $ 
create trigger chk_minutos before insert on filmes 
for each row 
begin
if new.minutos < 0 then 
set new.minutos = null;
end if;
end$

delimiter ;
```
## 1º Incluir insert;
```SQL
Inserir aqui

insert into Filmes (titulo, minutos) values ('The terrible trigger', 120);
insert into Filmes (titulo, minutos) values ('O alto da campadecida', 135);
insert into Filmes (titulo, minutos) values ('Faroeste Caboclo', 240);
insert into Filmes (titulo, minutos) values ('The matrix', 90);
insert into Filmes (titulo, minutos) values ('Blade runner ', -88);
insert into Filmes (titulo, minutos) values ('O labirinto do fauno', 110);
insert into Filmes (titulo, minutos) values ('Metropole', 0);
insert into Filmes (titulo, minutos) values ('A lista', 120);
```
![Captura de tela 2023-10-23 211657](https://github.com/WanderleiJullia/Trigger./assets/144744092/2189cf6a-9f30-4f38-8337-c8e86270a6c1)


## 2º Incluir Chk_Minutos; 
```SQL
Inserir aqui
delimiter $ 
create trigger chk_minutos before insert on Filmes
for each row 
begin
if new.minutos < 0 then 

signal sqlstate '45000'
set message_text = "Valor inváilido para minutos", 
mysql_errno = 2022; 

end if;
end$ 

delimiter ;
```
![image](https://github.com/WanderleiJullia/Trigger./assets/144744092/20dbdce9-a5c2-4347-9ed8-ae3d7e33f430)

## 3º Inserir Log_deletions 
```SQL
Inserir aqui
create table Log_deletion(
id  int  primary key  auto_increment,
titulo varchar(60),
quando datetime,
quem varchar(40)
);

delimiter $ 
create trigger Log_deletion after delete on Filmes 
for each row 
begin
insert into log_deletion values (null, old.titulo, sysdate(), user());
end$ 
delimiter ; 

delete from Filmes where id = 2;
delete from Filmes where id = 4;

select * from log_deletion;
```
![Filmes Log ](https://github.com/WanderleiJullia/Trigger./assets/144744092/fa96b0f5-b6ac-405b-8902-68136a35955a)



#Matheus Huank
Obrigado!! 
