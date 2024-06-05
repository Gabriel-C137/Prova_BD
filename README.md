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

```sql
CREATE DATABASE Mother_Base;
USE Mother_Base;


CREATE TABLE boss (
    id_boss								INT				primary key		identity,
    nome								VARCHAR(100)	NOT NULL,
    fk_titulo_titulo_pk					INT,

    FOREIGN KEY (fk_titulo_titulo_pk) REFERENCES titulo(id_titulo)
);

CREATE TABLE titulo (
    id_titulo		INT				PRIMARY KEY		identity,
    titulo			VARCHAR(100)	NOT NULL
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

CREATE TABLE habilidade (
    id_habilidade		INT				PRIMARY KEY			identity,
    habilidade			VARCHAR(1000)	NOT NULL
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

CREATE TABLE localizacao (
    id_localizacao			INT				PRIMARY KEY			identity,
    localizacao				VARCHAR(100)	NOT NULL
);

CREATE TABLE pedido (
    id_pedido					INT				PRIMARY KEY		identity,
    data						DATE			NOT NULL,
    descricao					VARCHAR(1000)	NOT NULL,
    status						VARCHAR(100)	NOT NULL,
    fk_cliente_id_cliente		INT,

    FOREIGN KEY (fk_cliente_id_cliente) REFERENCES cliente(id_cliente)
);

CREATE TABLE cliente (
    id_cliente				INT				PRIMARY KEY			identity,
    nome					VARCHAR(100)	NOT NULL,
    fk_contato_id_contato	INT,
    FOREIGN KEY (fk_contato_id_contato) REFERENCES contato(id_contato)
);

CREATE TABLE contato (
    id_contato		INT				PRIMARY KEY			identity,
    contato			VARCHAR(100)	NOT NULL
);



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

## 5°

## 6°

## 7°
