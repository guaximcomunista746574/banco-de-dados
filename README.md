# banco-de-dados
SELECT* FROM produtos

CREATE TABLE produtos (
id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(255),
	preco DECIMAL(10,2)
);
DROP TABLE produtos;

ALTER TABLE produtos ADD descricao TEXT;

INSERT INTO produtos (id ,nome,preco)VALUES
(1,'Nescau Radical',8.00),
(2, 'Yaukut Atctvia',4.00),
(3,'Alpino Amargo',5.00),
(4,'Miojo Monica',1.00 );

UPDATE produtos SET nome= 15.00 WHERE id=1;
UPDATE produtos SET nome='Toddy'WHERE nome LIKE 'Nescau Radicai';

UPDATE produtos SET descricao ='armazenar em ambiente gelado'
WHERE id=2 

UPDATE produtos SET descricao = 'promocao'

WHERE preco < 5.00;
--deletar dado de um tabela
DELETE FROM produtos WHERE nome LIKE 'Alpino Amargo';
DELETE FROM produtos WHERE id=1;

--comandos DQL (linguagem de consulta de dados)
--selecinonar todos (*) produtos ,ordene  ascendente
SELECT * FROM produtos ORDER BY ASC;

--ordenar pelo preço crescente (menor para o maior)
SELECT * FROM produtos ORDER BY preco ASC;

--ordenar pelo preço decresente (maior pra o menor)
SELECT * FROM produtos ORDER BY preco DESC;

--selecionar por colunas 
SELECT preco,nome FROM produtos ORDER BY id;

--selecionar produtos maior que 4.00
SELECT preco FROM produtos WHERE preco > 4.00;

--seleconar produto mais caro
SELECT MAX (preco ) AS titulo FROM produtos;
SELECT preco,nome FROM produtos ORDER BY valor DESC limit 1;

--simulando busca exato
SELECT * FROM produtos WHERE nome LIKE 'Nescau Radical';

CREATE TABLE funcionario(
id SERIAL PRIMARY KEY,
	nome VARCHAR (255) NOT NULL,
	data_nasc DATE,
	sexo CHAR (1),
	salario DECIMAL (10,2),
	ativo boolean
	);
	
	INSERT INTO funcionario (nome, data_nasc, sexo, salario, ativo) VALUES 
	
	('Bob','1990-05-15','M', 2000.00, true),
	('Augusto','1970-04-01', 'M' , 1500.00,false),
	 ('Joana', '2001-01-01', 'F', 1800.00, true),
	 ('Elisa', '1995-10-02' ,'F', 1900.00,true);
	SELECT * FROM funcionario;
	
	---seleciona funcionaris que nascem apos 01-01-1992
	SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';
	
	--seleciona  funcionaros  em um periodo 
	SELECT nome, data_nasc FROM funcionario
	WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';
	--Contabilizar
	
	SELECT sexo, COUNT (*), AVG (salario)AS media_salarial
	FROM funcionario GROUP BY sexo;

-- Criação da tabela de clientes
CREATE TABLE Clientes (
    Codigo INT PRIMARY KEY,
    Nome VARCHAR(255),
    Email VARCHAR(255)
);
SELECT * FROM clientes

-- Inserir 3 clientes
INSERT INTO Clientes (Codigo, Nome, Email) VALUES
    (1, 'Antonio', 'Antonio@gmail.com'),
    (2, 'angela', 'Angela2@gmail.com'),
    (3, 'fausto', 'Fausto@gmail.com');

-- Criação da tabela de itens
CREATE TABLE Itens (
    Codigo INT PRIMARY KEY,
    Descricao VARCHAR(255),
    Preco DECIMAL(10, 2)
);
SELECT * FROM itens
DROP TABLE itens

-- Inserir itens na tabela de itens
INSERT INTO Itens (Codigo, Descricao, Preco)
VALUES
    (101, 'maçã', 10.99),
    (102, 'uva', 15.99),
    (103, 'pera', 8.49);
	
DROP TABLE clientes

-- Deletar um item da tabela de itens (por exemplo, o item com código 101)
DELETE FROM Itens
WHERE Codigo = 101;

-- Remover um cliente da tabela de clientes (por exemplo, o cliente com código 1)
DELETE FROM Clientes
WHERE Codigo = 1;

-- Adicionar um novo cliente com UPDATE (por exemplo, atualizar o cliente com código 2)
UPDATE Clientes
SET Nome = 'alberto', Email = 'aalberto@gamil.com'
WHERE Codigo = 2;

-- Adicionar um novo item na tabela de itens com UPDATE (por exemplo, atualizar o item com código 102)
UPDATE Itens
SET Descricao = 'amendoim', Preco = 12.99
WHERE Codigo = 102;


-- Criação da tabela "cidades"
CREATE TABLE cidades (
    id_cidade SERIAL PRIMARY KEY,
    nome_cidade VARCHAR(50),
    cep CHAR(50),
    id_estado INT REFERENCES estados (id_estado)
);

-- Inserir dados na tabela "cidades"
INSERT INTO cidades (id_cidade, nome_cidade, cep, id_estado) VALUES
    (10, 'Nova Londrina', '8797-000', 1),
    (11, 'Marilena', '8796-000', 1),
    (12, 'Itauna', '8798-000', 1),
    (13, 'Primavera', '19273-000', 2),
    (14, 'Rosana', '19274-000', 2),
    (15, 'Cachoeira da Prata', '35765-000', 3);

-- Consulta para selecionar todas as cidades
SELECT * FROM cidades;

-- Consulta para selecionar todos os estados
SELECT * FROM estados;

-- Remoção da tabela "cidade" (tabela inexistente, não precisa ser removida)
-- DROP TABLE cidade;

-- Consulta para inserir estados na tabela "estados"
INSERT INTO estados (id_estado, nome_estado, sigla) VALUES
    (1, 'Paraná', 'PR'),
    (2, 'São Paulo', 'SP'),
    (3, 'Minas Gerais', 'MG');

-- Consulta para selecionar todos os estados
SELECT * FROM estados;

-- Consulta para selecionar todas as cidades e seus estados
SELECT cidades.nome_cidade, estados.nome_estado
FROM cidades
INNER JOIN estados ON cidades.id_estado = estados.id_estado
WHERE estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

-- Consulta para selecionar os estados com mais cidades
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados
INNER JOIN cidades ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

-- Criação da tabela "pessoa" para o relacionamento entre pessoas, cidades e estados
CREATE TABLE pessoa (
    id_pessoa SERIAL PRIMARY KEY,
    nome_pessoa VARCHAR(60),
    data_nascimento DATE,
    sexo CHAR(1),
    estado_civil VARCHAR(60),
    profissao VARCHAR(60),
    id_cidade INT REFERENCES cidades (id_cidade)
);

-- Inserir dados na tabela "pessoa"
INSERT INTO pessoa (id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade) VALUES
(1, 'Marcele', '2000-01-01', 'f', 'solteira', 'Arquiteta', 10),
(2, 'Ananias', '1980-02-20', 'm', 'casado', 'Carpinteiro', 10),
(3, 'Silvio', '1950-10-10', 'm', 'casado', 'Dublador', 11),
(4, 'Léo', '2001-01-02', 'm', 'casado', 'Entregador', 11),
(5, 'Chris', '1990-05-05', 'm', 'solteiro', 'Fisiculturista', 12),
(6, 'Ana', '1997-04-01', 'f', 'solteira', 'Cantora', 13),
(7, 'Jaime', '1970-03-01', 'm', 'casado', 'Carteiro', 14),
(8, 'Alice', '2002-01-10', 'f', 'solteira', 'Advogada', 15),
(9, 'Valentina', '1995-05-03', 'f', 'solteira', 'Zootecnista', 15),
(10, 'Laura', '1998-05-09', 'f', 'solteira', 'Balconista', 15);

-- Consulta para selecionar todos os dados da tabela "pessoa"
SELECT * FROM pessoa;

-- Consulta para selecionar pessoas com suas respectivas cidades
SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa
INNER JOIN cidades ON pessoa.id_cidade = cidades.id_cidade;

-- Consulta para selecionar pessoas com suas respectivas cidades e estados
SELECT pessoa.nome_pessoa, cidades.nome_cidade, estados.nome_estado
FROM pessoa
INNER JOIN cidades ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados ON cidades.id_estado = estados.id_estado;

-- Consulta para selecionar pessoas do estado do Paraná
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados
INNER JOIN cidades ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

-- Consulta para selecionar o nome da pessoa, sua profissão e a cidade onde ela reside
SELECT pessoa.nome_pessoa, pessoa.profissao, cidades.nome_cidade
FROM pessoa
INNER JOIN cidades ON pessoa.id_cidade = cidades.id_cidade;

-- Consulta para selecionar a cidade com mais homens ou mulheres
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades
INNER JOIN pessoa ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'm'
GROUP BY cidades.nome_cidade
ORDER BY Quantidade DESC;
--1 selecione todas as pessoas 
SELECT * FROM pessoa;

--2 selecione todas as pessoas do sexo masculini"m"
SELECT * FROM pessoa WHERE sexo = 'm';
SELECT * FROM pessoa WHERE sexo = 'f';

--3 selecione todas as pessoas com o estado civil solteiro;
SELECT * FROM pessoa WHERE estado_civil = 'solteira';
SELECT * FROM pessoa WHERE estado_civil = 'solteiro';


--4 selecione as pessoas e suas profissoes 
SELECT nome_pessoa, profissao FROM pessoa;

--5 selecione as pessoas que nasceram entre 1890-01-01 e 2001-01-01
SELECT * FROM pessoa WHERE data_nascimento BETWEEN '1980-01-01' AND '2001-01-01';

--6 selecione as pessoas do estado de são paulo
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados
INNER JOIN cidades ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado = 'São Paulo';






 
