
-- 1. Informe o SQL para criação da tabela aluno:

CREATE TABLE aluno(
        matricula 	INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
        nome 		VARCHAR(55),
        email 		VARCHAR(55),
        turma 		INTEGER(1)
);

-- 2. Informe o SQL para criação da tabela disciplina:

CREATE TABLE disciplina(	
	id_disciplina INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
	nome VARCHAR(255),
    carga_horaria INTEGER
 );
	 

-- 3. Informe o SQL para criação da tabela pauta:
--Nao feita
CREATE TABLE pauta(
    id_pauta        INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
	matricula		INTEGER NOT NULL,
	id_disciplina	INTEGER NOT NULL,
   	nota_1			DECIMAL,
	nota_2 	        DECIMAL,
 	nota_3          DECIMAL,
	FOREIGN KEY (matricula) 	REFERENCES aluno(matricula),
	FOREIGN KEY (id_disciplina) REFERENCES disciplina(id_disciplina)
);


--- 4. Com o comando ALTER TABLE mude o nome das colunas nota_1, nota_2 e nota_3 para avaliacao_1, avaliacao_2 e avaliacao_3 (2 pontos):

ALTER TABLE pauta 
RENAME COLUMN nota_1 TO avaliacao_1 ;

ALTER TABLE pauta
RENAME COLUMN nota_2 TO avaliacao_2;

ALTER TABLE pauta
RENAME COLUMN nota_3 TO avaliacao_3;

-----------------------------------
------- Inserção dos dados---------
-----------------------------------
-- Planilha com os dados a serem inseridos: https://tinyurl.com/y3ngdd5s

--1. Informe o SQL para inserção na tabela alunos:

INSERT INTO aluno(nome, email,turma)
VALUES
	  ('Ana Paula Silva', 'aps@residencia.com.br',1),
	  ('João Souza', 'js@residencia.com.br',1),
	  ('Maria Moreira', 'mm@residencia.com.br', 1),
	  ('Daiane Costa', 'dc@residencia.com.br', 2),
	  ('Guilherme Silva', 'gs@residencia.com.br', 1),
	  ('Julia Almeida', 'ja@residencia.com.br', 2),
	  ('Diogo Andrade', 'da@residencia.com.br',2),
	  ('Manuela Botelho', 'mb@residencia.com.br',1),
	  ('Thiago Tavares', 'tt@residencia.com.br',2),
	  ('João Pedro Carvalho', 'jpc@residencia.com.br',1);


--2. Informe o SQL para inserção na tabela disciplina:

INSERT INTO disciplina(nome, carga_horaria)
VALUES('Banco de dados',24),
	  ('Lógica de programação', 40),
	  ('Programação backend', 44);
	 
	 
	 
	 
--3. Informe o SQL para inserção dos seguintes dados na tabela pauta
-- (note que devem ser inseridos os respectivos identificadores de
--  alunos e disciplinas, não os nomes):

INSERT INTO pauta(matricula, id_disciplina,avaliacao_1, avaliacao_2, avaliacao_3) 
VALUES ((select matricula from aluno where nome like 'Ana Paula Silva'), (select id_disciplina from disciplina where nome like 'Banco de dados'), 10,9,10),
((select matricula from aluno where nome like 'Ana Paula Silva'), (select id_disciplina from disciplina where nome like 'Lógica de programação'), 9,8,7),
((select matricula from aluno where nome like 'Ana Paula Silva'), (select id_disciplina from disciplina where nome like 'Programação backend'), 7,7,9),
((select matricula from aluno where nome like 'João Souza'), (select id_disciplina from disciplina where nome like 'Banco de dados'), 9,6,7),
((select matricula from aluno where nome like 'João Souza'), (select id_disciplina from disciplina where nome like 'Lógica de programação'), 10,10,10),
((select matricula from aluno where nome like 'João Souza'), (select id_disciplina from disciplina where nome like 'Programação backend'), 9,8,9),
((select matricula from aluno where nome like 'Maria Moreira'), (select id_disciplina from disciplina where nome like 'Banco de dados'), 10,7,7),
((select matricula from aluno where nome like 'Daiane Costa'), (select id_disciplina from disciplina where nome like 'Lógica de programação'), 8,6,9),
((select matricula from aluno where nome like 'Daiane Costa'), (select id_disciplina from disciplina where nome like 'Programação backend'), 6,6,8),
((select matricula from aluno where nome like 'Guilherme Silva'), (select id_disciplina from disciplina where nome like 'Programação backend'), 8,6,9),
((select matricula from aluno where nome like 'Diogo Andrade'), (select id_disciplina from disciplina where nome like 'Banco de dados'), 8,8,10),
((select matricula from aluno where nome like 'Manuela Botelho'), (select id_disciplina from disciplina where nome like 'Lógica de programação'), 5,7,7),
((select matricula from aluno where nome like 'Thiago Tavares'), (select id_disciplina from disciplina where nome like 'Programação backend'), 5,5,4),
((select matricula from aluno where nome like 'Thiago Tavares'), (select id_disciplina from disciplina where nome like 'Lógica de programação'), 7,7,6),
((select matricula from aluno where nome like 'João Pedro Carvalho'), (select id_disciplina from disciplina where nome like 'Banco de dados'), 5,5,2)
	
			
	
	
----------------------------------
------Atualização dos dados ------
----------------------------------
	
	--1. Atualizar o e-mail da aluna Manuela Botelho para mb@residencia.com.br
	UPDATE aluno 
	SET email = 'mb_NOVO@residencia.com.br'
	WHERE matricula = 18;
		 
	--2. Atualizar a nota 3 do aluno João Pedro Carvalho em Banco de dados para 7
	UPDATE pauta
	SET avaliacao_3 = 7
	WHERE matricula = 20;
	 


-----------------------------------
-------------Consultas-------------
-----------------------------------


--1. Selecionar o nome e a turma dos alunos:

SELECT nome, turma FROM aluno;

--2. Selecionar a quantidade total de alunos cadastrados:

SELECT COUNT(nome) as TotalDeAlunos FROM aluno;

--3. Selecionar a quantidade total de alunos em cada disciplina:

SELECT d.nome, COUNT(matricula) as TotalDeAlunos 
FROM pauta p
inner join disciplina d on d.id_disciplina = p.id_disciplina
group by d.nome; 

--4. Selecionar o nome, disciplina e as três notas de cada aluno (usando INNER JOIN ou WHERE):

SELECT  a.nome, 
		d.nome ,
        p.avaliacao_1,
        p.avaliacao_2,
        p.avaliacao_3
FROM pauta p
INNER JOIN aluno a ON p.matricula = a.matricula
inner join disciplina d ON p.id_disciplina = d.id_disciplina;


--5. Selecionar o nome dos alunos e a quantidade de disciplinas que cada um cursa:

SELECT 	a.nome,
		COUNT(id_disciplina)
FROM pauta p
inner JOIN aluno a on p.matricula = a.matricula
GROUP by a.nome;
	

--6. Selecionar o nome, disciplina e a média das três notas de cada aluno:

SELECT a.nome,
		d.nome,
		((p.avaliacao_1 + p.avaliacao_2 + p.avaliacao_3)/3) as TotalMedia
FROM pauta p
INNER JOIN aluno a ON p.matricula = a.matricula
INNER JOIN disciplina d ON p.id_disciplina = d.id_disciplina
GROUP by a.nome;	

--7. Selecionar o nome, disciplina e a média das três notas dos alunos que tenham média menor que 6:

SELECT a.nome,
		d.nome,
		((p.avaliacao_1 + p.avaliacao_2 + p.avaliacao_3)/3) as TotalMedia 
FROM pauta p
INNER JOIN aluno a on p.matricula = a.matricula and TotalMedia < 6
INNER JOIN disciplina d on p.id_disciplina = d.id_disciplina
GROUP BY a.nome ;


--8. Selecionar o nome da disciplina e a média das 3 notas de todos os alunos para cada disciplina:

SELECT d.nome,
		avg(p.avaliacao_1) as Media1,
		avg(p.avaliacao_2) as Media2,
		avg(p.avaliacao_3) as Media3
FROM pauta p 
INNER JOIN disciplina d on p.id_disciplina = d.id_disciplina 
GROUP BY d.nome;
	
--9. Selecione o aluno com maior nota na avaliação 1 de banco de dados, mostrando com foi a nota

SELECT a.nome,
		MAX(p.avaliacao_1) as MaiorNota
FROM disciplina d, aluno a
INNER JOIN pauta p on p.matricula = a.matricula
GROUP BY p.id_disciplina=1;


