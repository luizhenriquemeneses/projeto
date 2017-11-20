# projeto
Create schema faculdade;
use faculdade;
/* Está na 1ª Forma normal*/
Create table Cursos (
Nome_curso text (100) not null,
id_curso int(10) not null,
id_disciplina int(10) not null,
Periodo   int (10),
Valor float (30),
Primary key (Id_curso),
Foreign key (id_disciplina) references disciplina
);
/*1ª Forna Normal*/
Create table disciplina(
id_curso int(30) not null,
id_disciplina int(30) not null,
nome_disciplina  text(100) not null,
id_func  int(30),
Primary key (id_disciplina),
Foreign key (id_curso) references Cursos,
Foreign key (id_func) references Funcionários
);
/*1ª Forna Normal*/
Create Table turma (
id_turma  int(30) not null,
id_disciplina  int(30) not null,
id_aluno  int(30),
id_func  int(30),
Primary key (id_turma),
Foreign key (id_disciplina) references disciplina,
Foreign key (id_aluno) references aluno
);

CREATE TABLE ActualizaCurso ( 
	id INT(11) NOT NULL AUTO_INCREMENT, 
	id_curso INT(11) NOT NULL, 
	modificadoem datetime DEFAULT NULL, 
	acao VARCHAR(50) DEFAULT NULL, 
	PRIMARY KEY (id) 
);

CREATE TABLE ActualizaTurma ( 
	id INT(11) NOT NULL AUTO_INCREMENT, 
	id_turma INT(11) NOT NULL, 
	modificadoem datetime DEFAULT NULL, 
	acao VARCHAR(50) DEFAULT NULL, 
	PRIMARY KEY (id) 
);

CREATE TABLE ActualizaDisciplina ( 
	id INT(11) NOT NULL AUTO_INCREMENT, 
	id_disciplina INT(11) NOT NULL, 
	modificadoem datetime DEFAULT NULL, 
	acao VARCHAR(50) DEFAULT NULL, 
	PRIMARY KEY (id) 
);
/*Inserts*/
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '110011', '110022', 'Ingles tecnico', '110033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '120011', '120022', 'portugues tecnico', '120033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '130011', '130022', 'espanhol tecnico', '130033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '140011', '140022', 'Cientista de Dados', '140033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '150011', '150022', 'Tecnologia em Banco de Dados', '150033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '160011', '160022', 'Engenharia da Computação', '160033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '170011', '170022', 'Sistemas para Internet', '170033');
Insert into disciplina (id_curso,id_disciplina,nome_disciplina,id_func  ) values ( '180011', '180022', 'Computação de Android', '180033');


Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('210011', '210021','210031','210041');
Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('220011', '220022','220032','220042');
Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('230011', '230023','230033','230043');
Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('240011', '240024','240034','230043');
Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('250011', '250025','250035','230045');
Insert into turma (id_turma, id_disciplina,id_aluno, id_func) values ('260011', '250026','250036','230046');

Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' computação de android', '330031', '330041', '8', '900');
Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' analista de suporte a micro', '330033', '300043', '8', '1100');
Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' analista de  computação de engenharia', '330034', '300044', '8', '1600');
Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' Análise e Desenvolvimento de Sistemas', '330035', '330045', '5', '850');
Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' Rede de computadores', '330037', '330047', '4', '600');
Insert into cursos (Nome_curso ,id_curso,id_disciplina,Periodo,Valor) values (' Sistemas de informação', '330030', '300040', '8', '1200');
/*Deletes*/
Delete from turma Where id_turma='260011';
Delete from disciplina Where id_disciplina='180022';
Delete from cursos Where id_curso='330037';
/*Updates*/
UPDATE cursos SET Nome_curso = 'Análise de Sistemas'
WHERE id_curso =330035;
UPDATE cursos SET valor=1200
WHERE id_curso =330033;
UPDATE disciplina SET nome_disciplina='Cloud computing'
WHERE id_disciplina =120022;
/*Views*/
create view curso_periodo
(Periodo, Nome_curso) as
select C.Periodo,C.Nome_curso
from Cursos as C;

create view disciplina_turma
(nome_disciplina,id_aluno) as
select D.nome_disciplina, T.id_aluno
from Disciplina as D left join Turma as T ON D.id_disciplina=T.id_disciplina;

create view turma_curso
(id_aluno,Nome_curso) as
select T.id_aluno, C.Nome_curso
from  Turma as T RIGHT JOIN Cursos as C ON T.id_disciplina=C.id_disciplina;

/*Selects*/
select*from cursos as C join disciplina as D  ;
select count(id_curso) from cursos;
Select avg (valor) from cursos;
select*from curso_periodo;
select*from disciplina_turma;
select*from turma_curso;
show triggers;
Select*from cursos;


/*Triggers*/
DELIMITER $$
CREATE TRIGGER Tg_ActualizaCurso 
AFTER INSERT ON cursos
FOR EACH ROW BEGIN
INSERT INTO ActualizaCurso (acao) values ('INSERT');
END$$
DELIMITER ;
   
DELIMITER $$
CREATE TRIGGER Tg_ActualizaTurma 
BEFORE update ON turma
FOR EACH ROW BEGIN
INSERT INTO actualizaturma
SET acao = 'update',
id_turma = OLD.id_turma; 
END$$
DELIMITER ;

DELIMITER $$
CREATE TRIGGER Tg_ActualizaDisciplina 
AFTER DELETE ON disciplina
FOR EACH ROW BEGIN
INSERT INTO ActualizaDisciplina
SET acao = 'update',
id_disciplina = OLD.id_disciplina; 
END$$
DELIMITER;

/*Store Procedures*/
DELIMITER $$
create procedure sp_turma (id_aluno int(30), id_disciplina int(30))
begin
if((S_Idaluno !='') && (S_Iddisciplina!='')) then
insert into disciplina (id_aluno, id_disciplina) values (S_Idaluno,S_Iddisciplina);
Else 
select 'Uma turma precisa necessariamente de uma disciplina e um curso' as Msg;
end if;
end;$$

DELIMITER $$
create procedure sp_curso (Nome_curso text(100), Valor float (30))
begin
if((S_Valor !='') && (S_Nomecurso!='')) then
insert into cursos (Valor, Nome_curso) values (S_Valor,S_Nomecurso);
Else 
select 'Um curso precisa de um nome e um Id único' as Msg;
end if;
end;$$

DELIMITER $$
create procedure sp_disciplina (nome_disciplina int(30))
begin
if((S_nomeDisciplina !='')) then
insert into disciplina (nome_disciplina) values (S_nomeDisciplina);
Else 
select 'Uma disciplina precisa necessariamente de um curso' as Msg;
end if;
end;$$


/*index*/
/*Esse índice é importante, pois com o passar do tempo o numero de turmas que passaram pela faculdade é bastante grande.
Um exemplo de situação é quando algum dia um certo aluno for á instituição e precisar saber qual era sua turma á anos atrás, com o index será mais fácil de saber.*/
create index IndiceTurma ON
turma(id_turma);

/*BLOB*/
CREATE TABLE IF NOT EXISTS SalvarImagens(
Idimagen INT (30) not null,
nome varchar (30),
tipo varchar (20),
dados longblob,
primary key(Idimagen)
);

Insert into SalvarImagens (Idimagen, nome, tipo, dados) values
('1','microsoft1','imagen jpg', load_file("C:\Users\Henrique\Pictures\Microsoft\microsoft1.jpg")),
('2','microsoft2','imagen jpg', load_file("C:\Users\Henrique\Pictures\Microsoft\microsoft2.jpg")),
('3','microsoft3','imagen jpg', load_file("C:\Users\Henrique\Pictures\Microsoft\microsoft3.jpg")),
('4','microsoft4','imagen PNG', load_file("C:\Users\Henrique\Pictures\Microsoft\microsoft4.png"));

select*from SalvarImagens;

show variables like "secure_file_priv";

select dados into outfile 'C:\wamp64\tmp\microsoft1.jpg'
from SalvarImagens
where Idimagen = 1;
