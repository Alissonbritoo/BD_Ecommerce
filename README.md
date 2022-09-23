Banco criado Comercio

-- Criação do banco de dados para o cenário de E-commerce
create database comercio;
use comercio;
drop table clients;
-- criar tabela cliente

-- recuperações simples em select só aplicar nas tabelas
SELECT Lname
FROM  clients
WHERE conditions;

-- filtrar em mysql aplicar nas tabelas
SELECT * FROM clients WHERE CPF = 12346789;

--  Ordenar em mysql aplicar nas tabelas
SELECT * FROM clients order by CPF asc;

-- filtros aos grupos – HAVING Statement aplicar para as tabelas
SELECT * FROM clients WHERE CPF group by adress having 99346789;


create table clients(
idClient int auto_increment primary key,
Fname varchar(10),
Minit char(3),
Lname varchar(20),
CPF char(11) not null,
Adress varchar (255),
constraint unique_client unique(CPF)
);


desc clients;
drop table product;
-- criar tabela produto
create table product(
idProduct int auto_increment primary key,
Pname varchar(255) not null,
classification_kids bool default false,
category enum('Eletrônico', 'Vestimenta', 'Brinquedos','Alimentos', 'Móveis')not null,
avaliação float default 0,
size varchar(10)
);
-- Continuar no desafio: Termine de implementar a tabela e crie a conexão com as tabelas necessárias
-- além disso, reflita essa modificação no diagrama de esquema relacional
-- criar constraints relacionadas ao pagamento.
create table payments(
idclient int,
idpayment int, 
typePayment enum('Boleto','Cartão', 'Dois cartões'),
limitAvailable float,
primary key(idClient, id_payment)
);

desc orders;
-- criar tabela pedido
create table orders (
idOrder int auto_increment primary key,
idOrderClient int,
orderStatus enum('Cancelado','Confirmado','Em processamento') default 'Em processamento',
orderDescription varchar(255),
sendValue float default 10,
paymentCash boolean default false, 
constraint fk_orders_client foreign key (idOrderClient) references client(idClient)
on update cascade
);

-- Criar table estoque
create table productStorage(
idProdStorage int auto_increment primary key,
storageLocation varchar(255),
quantity int default 0
);

-- Criar tabela fornecedor
create table supplier (
idSupplier int auto_increment primary key,
SocialName varchar(255),
CNPJ char(15) not null,
contact char(11) not null,
constraint unique_supplier unique (CNPJ)
);
desc supplier;

-- Criar tabela vendedor
create table seller (
idSeller int auto_increment primary key,
SocialName varchar (255) not null,
AbstName varchar (255),
CNPJ char(15),
CPF char(9),
location varchar(255),
contact char (11) not null,
constraint unique_cnpj_seller unique (CNPJ),
constraint unique_cpf_seller unique (CPF)
);
desc productSeller;
create table productSeller (
idPseller int ,
idPproduct int ,
prodQuantity int default 1,
primary key(idPseller, idPproduct),
constraint fk_product_seller foreign key (idPseller) references seller(idSeller),
constraint fk_product_seller foreign key (idPproduct) references product (idProduct)
); 
create table productOrder(
idPOproduct int,
idPOorder int,
poQuantity int default 1,
poStatus enum('Disponível', 'Sem estoque') default 'Disponível',
primary key (idPOproduct, idPOorder),
constraint fk_productorder_seller foreign key (idPOproduct) references product(idProduct),
constraint fk_productorder_product foreign key (idPOorder) references orders(idOrder)
);
drop table storageLocation;
create table storageLocation(
idLproduct int,
idLstorage int,
location varchar (255) not null,
primary key (idLproduct, idLstorage),
constraint fk_storage_location_product foreign key (idLproduct) references product(idProduct),
constraint fk_storage_location_storage foreign key (idLstorage) references productStorage(idProdStorage)
);

create table productSupplier (
idPsSupplier int,
idPsProduct int,
quantity int not null,
primary key (idPsSupplier, idPsproduct),
constraint fk_storage_supplier_supplier foreign key (idPsSupplier) references supplier(idSupplier),
constraint fk_product_supplier_product foreign key (idPsProduct) references productStorage(idProduct)
);
show tables;
show databases;
use information_schema;
show tables;

desc referential_constraints;
select * from referential_constraints where constraint_schema = 'ecommerce';
-- idClient, Fname, Minit, Lname, CPF, adress
insert into clients (Fname, Minit, Lname, CPF, Adress) values
('Javier', 'M', 'Silva', '12346789', 'rua silva de prata 29, Carangola Cidades das flores'),
('Matheus', 'O', 'Pimentel', '98346789', 'rua boliche de prata 27 Centro - Cidades das flores'),
('Marcos', 'H', 'Silva', '99346789', 'rua banhasul de prata 22, centro Cidades das flores'),
('Mara', 'I', 'Salviana', '22346789', 'rua crypton de prata 26, carangola - cidades das flores');

-- idProduct, Pname, classification_kids boolean, category('Eletrônico', 'Vestimenta', 'Brinquedos', 'Alimentos', 'Móveis'), avaliação, size
 insert into product (Pname, classification_kids, category, avaliação, size) values
					 ('Fone de ouvido', false, 'Eletrônico', '4',null),
                      ('Barbie Elsa', true , 'Eletrônico', '3',null),
                      ('Body Carters', true, 'vestimenta', '5',null),
					  ('Microfone Vedo - Youtuber',False, 'Eletrônico','4', null),
                      ('Sofá retrátil', False, 'Móveis', '3', '3x57x80'),
                      ('Farinha de arroz', False, 'Alimentos', '2', null),
                      ('Fire Stick Amazon', False, 'Eletrônico', '3', null);
      select* from clients;
      select * from product;
      
   
