<div align=center ><h1> Prova Modelagem de Banco de Dados Completo </h1></div>

<br>
<div align=center >Cenário criado baseado em Metal Gear Solid 5: Phantom Pain</div>
<br>
<br>
Mother Base instalação do jogo: 

![Mother_Base_IMG](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/71dd42a7-7b91-4316-b21a-b021114b4e68)


## 1° Cenário 

  Você foi contratado por Big Boss dono de uma organização chamada Diamond Dogs, ele quer um banco de dados para a administrar de uma forma 
mais dinâmica sua base, a Mother Base, ele dá entrada/registra novos soldados, estes soldados enviados para diferentes unidades dependendo de suas 
habilidades, além disso é executado missões de expedição para adquirir recursos pras unidades e atender pedidos de clientes que podem solicitar 
uma missão. As unidades desenvolvem projetos e possuem recursos.

## 2° Modelo Conceitual

![Print Mother_Base](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/a3a50d5d-61e8-4260-ad22-5bb7ca09e541)

## 3° Modelo Lógico

![Log_Mother_Base_Current](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/abeaac4e-b10a-4f51-9824-99de1514912a)

## 4° Modelagem Física 

#### Criando o banco de dados:

```sql
CREATE DATABASE Mother_base;

USE Mother_base;
```

#### Criando as tabelas das entidades:

```sql
CREATE TABLE titulo (
    id_titulo		INT				PRIMARY KEY		identity,
    titulo			VARCHAR(100)	NOT NULL
);

CREATE TABLE boss (
    id_boss								INT				primary key		identity,
    nome								VARCHAR(100)	NOT NULL,
    fk_titulo_titulo_pk					INT,

    FOREIGN KEY (fk_titulo_titulo_pk) REFERENCES titulo(id_titulo)
);

CREATE TABLE habilidade (
    id_habilidade		INT				PRIMARY KEY			identity,
    habilidade			VARCHAR(1000)	NOT NULL
);

CREATE TABLE soldados (
    id_soldado						INT				PRIMARY KEY			identity,
    nome							VARCHAR(100)	NOT NULL,
    cargo							VARCHAR(200)	NOT NULL,
    pais							VARCHAR(100),
    status							VARCHAR(200)	NOT NULL,
    idade							INT,
    data							DATE,
    fk_habilidade_habilidade_pk		INT,
    FOREIGN KEY (fk_habilidade_habilidade_pk) REFERENCES habilidade(id_habilidade)
);


CREATE TABLE unidades (
    id_unidade				INT					PRIMARY KEY			identity,
    nome					VARCHAR(100)		NOT NULL,
    tipo					VARCHAR(100)		NOT NULL,
    fk_boss_id_boss			INT,
    FOREIGN KEY (fk_boss_id_boss) REFERENCES boss(id_boss)
);

CREATE TABLE recursos (
    id_recurso				INT				PRIMARY KEY  identity,
    tipo					VARCHAR(100)	NOT NULL,
    quantidade				FLOAT			NOT NULL
);

CREATE TABLE projetos (
    id_projeto				INT					PRIMARY KEY			identity,
    nome					VARCHAR(100)		NOT NULL,
    descricao				VARCHAR(2000)		NOT NULL,
    status					VARCHAR(50)			NOT NULL,
    fk_unidades_id_unidade	INT,

    FOREIGN KEY (fk_unidades_id_unidade) REFERENCES unidades(id_unidade)
);

CREATE TABLE localizacao (
    id_localizacao			INT				PRIMARY KEY			identity,
    localizacao				VARCHAR(100)	NOT NULL
);

CREATE TABLE contato (
    id_contato		INT				PRIMARY KEY			identity,
    contato			VARCHAR(100)	NOT NULL
);

CREATE TABLE cliente (
    id_cliente				INT				PRIMARY KEY			identity,
    nome					VARCHAR(100)	NOT NULL,
    fk_contato_id_contato	INT,
    FOREIGN KEY (fk_contato_id_contato) REFERENCES contato(id_contato)
);

CREATE TABLE pedido (
    id_pedido					INT				PRIMARY KEY		identity,
    data						DATE			NOT NULL,
    descricao					VARCHAR(1000)	NOT NULL,
    status						VARCHAR(100)	NOT NULL,
    fk_cliente_id_cliente		INT,

    FOREIGN KEY (fk_cliente_id_cliente) REFERENCES cliente(id_cliente)
);



CREATE TABLE missao (
    id_missao						INT				PRIMARY KEY			identity,
    nome							VARCHAR(100)	NOT NULL,
    data							DATE			NOT NULL,
    status							VARCHAR(100)	NOT NULL,
    fk_localizacao_id_localizacao	INT,
    fk_pedido_id_pedido				INT,

    FOREIGN KEY (fk_localizacao_id_localizacao) REFERENCES localizacao(id_localizacao),
    FOREIGN KEY (fk_pedido_id_pedido) REFERENCES pedido(id_pedido)
);

```

#### Criando as tabelas das relações:

```sql

CREATE TABLE pertenca (
    fk_soldados_id_soldado INT,
    fk_unidades_id_unidade INT,
    PRIMARY KEY (fk_soldados_id_soldado, fk_unidades_id_unidade),
    FOREIGN KEY (fk_soldados_id_soldado) REFERENCES soldados(id_soldado),
    FOREIGN KEY (fk_unidades_id_unidade) REFERENCES unidades(id_unidade)
);

CREATE TABLE possua (
    fk_unidades_id_unidade INT,
    fk_recursos_id_recurso INT,
    PRIMARY KEY (fk_unidades_id_unidade, fk_recursos_id_recurso),
    FOREIGN KEY (fk_unidades_id_unidade) REFERENCES unidades(id_unidade),
    FOREIGN KEY (fk_recursos_id_recurso) REFERENCES recursos(id_recurso)
);

CREATE TABLE Executa (
    fk_soldados_id_soldado INT,
    fk_missao_id_missao INT,
    PRIMARY KEY (fk_soldados_id_soldado, fk_missao_id_missao),
    FOREIGN KEY (fk_soldados_id_soldado) REFERENCES soldados(id_soldado),
    FOREIGN KEY (fk_missao_id_missao) REFERENCES missao(id_missao)
);

CREATE TABLE faz (
    fk_pedido_id_pedido INT,
    fk_cliente_id_cliente INT,
    PRIMARY KEY (fk_pedido_id_pedido, fk_cliente_id_cliente),
    FOREIGN KEY (fk_pedido_id_pedido) REFERENCES pedido(id_pedido),
    FOREIGN KEY (fk_cliente_id_cliente) REFERENCES cliente(id_cliente)
);

CREATE TABLE faz2 (
    fk_unidades_id_unidade INT,
    fk_projetos_id_projeto INT,
    PRIMARY KEY (fk_unidades_id_unidade, fk_projetos_id_projeto),
    FOREIGN KEY (fk_unidades_id_unidade) REFERENCES unidades(id_unidade),
    FOREIGN KEY (fk_projetos_id_projeto) REFERENCES projetos(id_projeto)
);
```

## 5° Inserção de dados

### Inserindo 20 dados distintos dentro de cada tabelas:

```sql
INSERT INTO titulo VALUES ('Commander');
INSERT INTO titulo VALUES ('General');
INSERT INTO titulo VALUES ('Lieutenant');
INSERT INTO titulo VALUES ('Sergeant');
INSERT INTO titulo VALUES ('Captain');
INSERT INTO titulo VALUES ('Colonel');
INSERT INTO titulo VALUES ('Major');
INSERT INTO titulo VALUES ('Lieutenant Colonel');
INSERT INTO titulo VALUES ('Chief Warrant Officer');
INSERT INTO titulo VALUES ('Warrant Officer');
INSERT INTO titulo VALUES ('First Lieutenant');
INSERT INTO titulo VALUES ('Second Lieutenant');
INSERT INTO titulo VALUES ('Master Sergeant');
INSERT INTO titulo VALUES ('Staff Sergeant');
INSERT INTO titulo VALUES ('Private First Class');
INSERT INTO titulo VALUES ('Private');
INSERT INTO titulo VALUES ('Specialist');
INSERT INTO titulo VALUES ('Corporal');
INSERT INTO titulo VALUES ('Sergeant First Class');
INSERT INTO titulo VALUES ('Lance Corporal');

INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Big Boss', 1);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Kazuhira Miller', 2);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Ocelot', 3);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Huey Emmerich', 4);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Quiet', 5);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Venom Snake', 6);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Eli', 7);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Code Talker', 8);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Skull Face', 9);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Tretij Rebenok', 10);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Paz Ortega', 11);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Chico', 12);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Strangelove', 13);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The Boss', 14);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The Sorrow', 15);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The Pain', 16);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The Fear', 17);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The End', 18);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('The Fury', 19);
INSERT INTO boss (nome, fk_titulo_titulo_pk) VALUES ('Paramedic', 20);

INSERT INTO habilidade (habilidade) VALUES ('Marksmanship');
INSERT INTO habilidade (habilidade) VALUES ('Medical Expertise');
INSERT INTO habilidade (habilidade) VALUES ('Engineering');
INSERT INTO habilidade (habilidade) VALUES ('Infantry Tactics');
INSERT INTO habilidade (habilidade) VALUES ('Scouting');
INSERT INTO habilidade (habilidade) VALUES ('Driving');
INSERT INTO habilidade (habilidade) VALUES ('Gunnery');
INSERT INTO habilidade (habilidade) VALUES ('Piloting');
INSERT INTO habilidade (habilidade) VALUES ('Demolition');
INSERT INTO habilidade (habilidade) VALUES ('Logistics');
INSERT INTO habilidade (habilidade) VALUES ('Command');
INSERT INTO habilidade (habilidade) VALUES ('Espionage');
INSERT INTO habilidade (habilidade) VALUES ('Reconnaissance');
INSERT INTO habilidade (habilidade) VALUES ('Heavy Weapons');
INSERT INTO habilidade (habilidade) VALUES ('Cyber Warfare');
INSERT INTO habilidade (habilidade) VALUES ('Chemical Warfare');
INSERT INTO habilidade (habilidade) VALUES ('Marksmanship');
INSERT INTO habilidade (habilidade) VALUES ('Medical Expertise');
INSERT INTO habilidade (habilidade) VALUES ('Engineering');
INSERT INTO habilidade (habilidade) VALUES ('Infantry Tactics');

INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 1', 'Sniper', 'USA', 'Active', 25, '2023-01-01', 1);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 2', 'Medic', 'Canada', 'Active', 30, '2023-02-01', 2);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 3', 'Engineer', 'UK', 'Active', 28, '2023-03-01', 3);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 4', 'Infantry', 'France', 'Active', 26, '2023-04-01', 4);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 5', 'Scout', 'Germany', 'Active', 27, '2023-05-01', 5);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 6', 'Driver', 'USA', 'Active', 29, '2023-06-01', 6);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 7', 'Gunner', 'Canada', 'Active', 31, '2023-07-01', 7);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 8', 'Pilot', 'UK', 'Active', 32, '2023-08-01', 8);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 9', 'Demolitions', 'France', 'Active', 33, '2023-09-01', 9);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 10', 'Logistics', 'Germany', 'Active', 34, '2023-10-01', 10);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 11', 'Commando', 'USA', 'Active', 35, '2023-11-01', 11);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 12', 'Spy', 'Canada', 'Active', 36, '2023-12-01', 12);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 13', 'Recon', 'UK', 'Active', 37, '2024-01-01', 13);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 14', 'Heavy Weapons', 'France', 'Active', 38, '2024-02-01', 14);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 15', 'Cyber Warfare', 'Germany', 'Active', 39, '2024-03-01', 15);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 16', 'Chemical Warfare', 'USA', 'Active', 40, '2024-04-01', 16);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 17', 'Sniper', 'Canada', 'Active', 41, '2024-05-01', 17);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 18', 'Medic', 'UK', 'Active', 42, '2024-06-01', 18);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 19', 'Engineer', 'France', 'Active', 43, '2024-07-01', 19);
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) VALUES ('Soldado 20', 'Infantry', 'Germany', 'Active', 44, '2024-08-01', 20);

INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Combat Unit', 'Combat', 1);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Medical Unit', 'Support', 2);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Engineering Unit', 'Support', 3);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Intel Unit', 'Support', 4);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Base Development Unit', 'Development', 5);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Security Team', 'Defense', 6);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('R&D Team', 'Development', 7);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Transport Team', 'Logistics', 8);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Supply Unit', 'Logistics', 9);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Recon Unit', 'Intel', 10);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Heavy Weapons Unit', 'Combat', 11);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Cyber Warfare Unit', 'Support', 12);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Chemical Warfare Unit', 'Support', 13);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Sniper Unit', 'Combat', 14);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Special Forces Unit', 'Combat', 15);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Infantry Unit', 'Combat', 16);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Logistics Unit', 'Logistics', 17);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Gunnery Unit', 'Combat', 18);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Piloting Unit', 'Support', 19);
INSERT INTO unidades (nome, tipo, fk_boss_id_boss) VALUES ('Demolition Unit', 'Combat', 20);

INSERT INTO recursos (tipo, quantidade) VALUES ('Ammunition', 5000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Medical Supplies', 2000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Food', 10000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Fuel', 8000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Weapons', 3000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Vehicles', 150);
INSERT INTO recursos (tipo, quantidade) VALUES ('Construction Materials', 10000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Electronics', 5000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Communication Equipment', 3000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Surveillance Equipment', 2000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Uniforms', 5000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Explosives', 1000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Fuel', 7000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Medical Supplies', 3000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Food', 9000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Weapons', 4000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Vehicles', 200);
INSERT INTO recursos (tipo, quantidade) VALUES ('Construction Materials', 11000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Electronics', 6000);
INSERT INTO recursos (tipo, quantidade) VALUES ('Communication Equipment', 4000);

INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Alpha', 'Develop new stealth technology', 'In Progress', 1);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Bravo', 'Improve medical response unit', 'Completed', 2);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Charlie', 'Upgrade base security', 'In Progress', 3);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Delta', 'Expand logistics capacity', 'Not Started', 4);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Echo', 'Develop new weapons', 'In Progress', 5);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Foxtrot', 'Enhance cyber warfare capabilities', 'Not Started', 6);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Golf', 'Improve communication systems', 'Completed', 7);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Hotel', 'Upgrade surveillance equipment', 'In Progress', 8);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project India', 'Develop new transport vehicles', 'Not Started', 9);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Juliet', 'Enhance training programs', 'Completed', 10);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Kilo', 'Expand base infrastructure', 'In Progress', 11);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Lima', 'Develop new reconnaissance technology', 'In Progress', 12);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Mike', 'Improve heavy weapons', 'In Progress', 13);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project November', 'Enhance chemical warfare capabilities', 'Not Started', 14);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Oscar', 'Improve infantry tactics', 'Completed', 15);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Papa', 'Develop new pilot training programs', 'In Progress', 16);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Quebec', 'Upgrade demolition equipment', 'Not Started', 17);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Romeo', 'Enhance sniper capabilities', 'In Progress', 18);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Sierra', 'Improve special forces tactics', 'Completed', 19);
INSERT INTO projetos (nome, descricao, status, fk_unidades_id_unidade) VALUES ('Project Tango', 'Develop new logistics software', 'In Progress', 20);

INSERT INTO localizacao (localizacao) VALUES ('Afghanistan');
INSERT INTO localizacao (localizacao) VALUES ('Africa');
INSERT INTO localizacao (localizacao) VALUES ('Central America');
INSERT INTO localizacao (localizacao) VALUES ('South America');
INSERT INTO localizacao (localizacao) VALUES ('Asia');
INSERT INTO localizacao (localizacao) VALUES ('Europe');
INSERT INTO localizacao (localizacao) VALUES ('North America');
INSERT INTO localizacao (localizacao) VALUES ('Middle East');
INSERT INTO localizacao (localizacao) VALUES ('Australia');
INSERT INTO localizacao (localizacao) VALUES ('Antarctica');
INSERT INTO localizacao (localizacao) VALUES ('Greenland');
INSERT INTO localizacao (localizacao) VALUES ('Iceland');
INSERT INTO localizacao (localizacao) VALUES ('New Zealand');
INSERT INTO localizacao (localizacao) VALUES ('Pacific Ocean');
INSERT INTO localizacao (localizacao) VALUES ('Indian Ocean');
INSERT INTO localizacao (localizacao) VALUES ('Arctic Ocean');
INSERT INTO localizacao (localizacao) VALUES ('Atlantic Ocean');
INSERT INTO localizacao (localizacao) VALUES ('Sahara Desert');
INSERT INTO localizacao (localizacao) VALUES ('Amazon Rainforest');
INSERT INTO localizacao (localizacao) VALUES ('Himalayas');

INSERT INTO contato (contato) VALUES ('contact1@example.com');
INSERT INTO contato (contato) VALUES ('contact2@example.com');
INSERT INTO contato (contato) VALUES ('contact3@example.com');
INSERT INTO contato (contato) VALUES ('contact4@example.com');
INSERT INTO contato (contato) VALUES ('contact5@example.com');
INSERT INTO contato (contato) VALUES ('contact6@example.com');
INSERT INTO contato (contato) VALUES ('contact7@example.com');
INSERT INTO contato (contato) VALUES ('contact8@example.com');
INSERT INTO contato (contato) VALUES ('contact9@example.com');
INSERT INTO contato (contato) VALUES ('contact10@example.com');
INSERT INTO contato (contato) VALUES ('contact11@example.com');
INSERT INTO contato (contato) VALUES ('contact12@example.com');
INSERT INTO contato (contato) VALUES ('contact13@example.com');
INSERT INTO contato (contato) VALUES ('contact14@example.com');
INSERT INTO contato (contato) VALUES ('contact15@example.com');
INSERT INTO contato (contato) VALUES ('contact16@example.com');
INSERT INTO contato (contato) VALUES ('contact17@example.com');
INSERT INTO contato (contato) VALUES ('contact18@example.com');
INSERT INTO contato (contato) VALUES ('contact19@example.com');
INSERT INTO contato (contato) VALUES ('contact20@example.com');

INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 1', 1);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 2', 2);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 3', 3);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 4', 4);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 5', 5);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 6', 6);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 7', 7);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 8', 8);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 9', 9);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 10', 10);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 11', 11);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 12', 12);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 13', 13);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 14', 14);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 15', 15);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 16', 16);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 17', 17);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 18', 18);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 19', 19);
INSERT INTO cliente (nome, fk_contato_id_contato) VALUES ('Cliente 20', 20);

INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-01-01', 'Supply drop', 'Completed', 1);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-02-01', 'Recon mission', 'In Progress', 2);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-03-01', 'Base construction', 'Not Started', 3);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-04-01', 'Intel gathering', 'Completed', 4);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-05-01', 'Equipment upgrade', 'In Progress', 5);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-06-01', 'Training program', 'Not Started', 6);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-07-01', 'Medical supplies', 'Completed', 7);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-08-01', 'Vehicle maintenance', 'In Progress', 8);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-09-01', 'Weapon testing', 'Not Started', 9);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-10-01', 'Security upgrade', 'Completed', 10);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-11-01', 'Base expansion', 'In Progress', 11);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2023-12-01', 'Communication setup', 'Not Started', 12);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-01-01', 'Surveillance mission', 'Completed', 13);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-02-01', 'Supply convoy', 'In Progress', 14);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-03-01', 'Cyber security', 'Not Started', 15);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-04-01', 'Chemical testing', 'Completed', 16);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-05-01', 'Combat simulation', 'In Progress', 17);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-06-01', 'Demolition operation', 'Not Started', 18);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-07-01', 'Recon mission', 'Completed', 19);
INSERT INTO pedido (data, descricao, status, fk_cliente_id_cliente) VALUES ('2024-08-01', 'Medical aid', 'In Progress', 20);

INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Nightfall', '2024-01-01', 'Completed', 1, 1);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Dawn', '2024-02-01', 'In Progress', 2, 2);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Sunset', '2024-03-01', 'Not Started', 3, 3);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Eclipse', '2024-04-01', 'Completed', 4, 4);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Midnight', '2024-05-01', 'In Progress', 5, 5);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Twilight', '2024-06-01', 'Not Started', 6, 6);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Dusk', '2024-07-01', 'Completed', 7, 7);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Zenith', '2024-08-01', 'In Progress', 8, 8);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Horizon', '2024-09-01', 'Not Started', 9, 9);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Daybreak', '2024-10-01', 'Completed', 10, 10);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Starlight', '2024-11-01', 'In Progress', 11, 11);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Moonbeam', '2024-12-01', 'Not Started', 12, 12);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation New Dawn', '2025-01-01', 'Completed', 13, 13);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Ember', '2025-02-01', 'In Progress', 14, 14);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Inferno', '2025-03-01', 'Not Started', 15, 15);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Flame', '2025-04-01', 'Completed', 16, 16);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Blaze', '2025-05-01', 'In Progress', 17, 17);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Radiance', '2025-06-01', 'Not Started', 18, 18);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Shimmer', '2025-07-01', 'Completed', 19, 19);
INSERT INTO missao (nome, data, status, fk_localizacao_id_localizacao, fk_pedido_id_pedido) VALUES ('Operation Glow', '2025-08-01', 'In Progress', 20, 20);

-- RELAÇÕES

INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (1, 1);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (2, 2);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (3, 3);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (4, 4);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (5, 5);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (6, 6);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (7, 7);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (8, 8);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (9, 9);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (10, 10);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (11, 11);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (12, 12);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (13, 13);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (14, 14);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (15, 15);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (16, 16);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (17, 17);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (18, 18);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (19, 19);
INSERT INTO pertenca (fk_soldados_id_soldado, fk_unidades_id_unidade) VALUES (20, 20);

INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (1, 1);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (2, 2);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (3, 3);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (4, 4);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (5, 5);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (6, 6);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (7, 7);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (8, 8);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (9, 9);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (10, 10);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (11, 11);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (12, 12);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (13, 13);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (14, 14);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (15, 15);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (16, 16);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (17, 17);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (18, 18);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (19, 19);
INSERT INTO possua (fk_unidades_id_unidade, fk_recursos_id_recurso) VALUES (20, 20);

INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (1, 1);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (2, 2);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (3, 3);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (4, 4);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (5, 5);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (6, 6);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (7, 7);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (8, 8);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (9, 9);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (10, 10);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (11, 11);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (12, 12);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (13, 13);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (14, 14);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (15, 15);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (16, 16);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (17, 17);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (18, 18);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (19, 19);
INSERT INTO Executa (fk_soldados_id_soldado, fk_missao_id_missao) VALUES (20, 20);

INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (1, 1);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (2, 2);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (3, 3);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (4, 4);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (5, 5);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (6, 6);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (7, 7);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (8, 8);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (9, 9);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (10, 10);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (11, 11);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (12, 12);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (13, 13);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (14, 14);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (15, 15);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (16, 16);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (17, 17);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (18, 18);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (19, 19);
INSERT INTO faz (fk_pedido_id_pedido, fk_cliente_id_cliente) VALUES (20, 20);

INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (1, 1);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (2, 2);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (3, 3);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (4, 4);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (5, 5);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (6, 6);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (7, 7);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (8, 8);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (9, 9);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (10, 10);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (11, 11);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (12, 12);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (13, 13);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (14, 14);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (15, 15);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (16, 16);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (17, 17);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (18, 18);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (19, 19);
INSERT INTO faz2 (fk_unidades_id_unidade, fk_projetos_id_projeto) VALUES (20, 20);
```

## 6° CRUD na Prática

### C - Create (Insert)  | Criar (Inserir)
### R - Read (Select)    | Ler (Selecionar)
### U - Update           | Atualizar
### D - Delete           | Excluir

#### Vamos começar vendo uma tabela: 

```sql
SELECT * FROM soldados;
```
![Captura de tela 2024-06-07 004153](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/b336a738-7eb5-45fe-b9bf-4810573489c3)

<hr>

#### C - Create (Insert) - Vou inserir uma linha nessa tabela:

```sql
INSERT INTO soldados (nome, cargo, pais, status, idade, data, fk_habilidade_habilidade_pk) 
	VALUES ('Soldado russo', 'Interprete', 'Russia', 'Active', 32, '2024-06-06', 1);
```

Para ver a tabela novamente é só rodar a função novamente - ``` SELECT * FROM soldados; ```
<br>
![Captura de tela 2024-06-07 004939](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/ebb04b82-f77d-4621-b9fb-2997cf6221d1)

#### Você também pode inserir dessa forma: 

```sql
INSERT INTO soldados
	VALUES ('Soldado Novo', 'Recruta', 'Brazil', 'Active', 22, '2024-06-07', 5);
```
![Captura de tela 2024-06-07 010325](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/4bb428cf-6869-4283-900e-ade97b74ec00)

<hr>

#### R - Read (Select)

#### Mostrando todos os dados dessa tabela:

```sql
SELECT * FROM boss;
```
![Captura de tela 2024-06-07 005313](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/863cafab-5f3c-45a4-b41e-e803dac6039a)


#### Posso puxei duas colunas de dados:

```sql
SELECT id_boss, nome FROM boss;
```
![Captura de tela 2024-06-07 005404](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/87d80d82-ba73-489b-87f9-feb522b1f72c)


#### Agora puxei apenas três colunas de dados:

```sql
SELECT nome, data , fk_localizacao_id_localizacao FROM missao;
```
![Captura de tela 2024-06-07 005515](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/0ba103b4-1cfa-4e5b-97b8-b0692f8e2aba)

<hr>

#### U - Update 

#### Nessa etapa utilizamos essa função para atulizar dados, Utilizando:

```sql
UPDATE soldados SET nome = 'New soldier' WHERE nome = 'Soldado Novo'
```
Antes:<br>
![Captura de tela 2024-06-07 015214](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/ecc51dba-d94b-4cc2-930e-868d87d61748)

Depois da atualização:<br>
![Captura de tela 2024-06-07 010632](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/c89b958c-1e23-4aed-aa87-ee1871a4eb88)



#### Atualizar até mesmo um campo especifico:

```sql
UPDATE soldados SET cargo = 'Sniper' WHERE cargo = 'Interprete' ; 
```
Transforme os cargo de Interpretes em Snipers <br>
Antes:<br>
![Captura de tela 2024-06-07 010325](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/8970d428-3dc8-436f-8df0-fe874ac44105)

Depois da atualização:<br>
![Captura de tela 2024-06-07 011044](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/0bb1fd76-1455-4a45-90af-52a9ae08f046)

<hr>

#### D - Delete

#### O simples e clássico, deletar 
#### Aqui deletamos dados de uma tabela ou até mesmo uma tabela inteira

```sql
DELETE FROM soldados WHERE id_soldado = 21;
```

Tabela Cheia, antes: <br>
![Captura de tela 2024-06-07 004939](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/bf7f9630-f6a8-4934-9bc1-1e2a95677535)

Tabela pós o Delete, id_soldado deletado é o 21: <br>
![Captura de tela 2024-06-07 011228](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/1f5a027e-8fbb-453b-8ec0-07e02a14522f)

#### Deletaremos mais um:

```sql
DELETE FROM soldados WHERE nome = 'New soldier';
```
![Captura de tela 2024-06-07 011405](https://github.com/Gabriel-C137/Prova_BD/assets/91295561/6b1a9c9a-5f72-4df2-9eda-fe56096104b0)

obs: Cuidado com o ```DELETE * FROM soldados``` !!!
<hr>

## 7°



















