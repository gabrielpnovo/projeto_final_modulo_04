O objetivo desse documento é descrever todas as informações relacionadas ao banco de dados que deverá ser criado para essa aplicação.

No projeto será utilizado o modelo relacional de banco de dados.

Segue abaixo estrutura das tabelas e o que cada uma irá armazenar. A ordem em que as tabelas são apresentadas representa também a ordem em que devem ser criadas para garantir que não haja conflito entre as relações.

1. Tabela "PARAMETROS":
Objetivo: Armazenar todos os parâmetros que serão utilizados no formulário de avaliação.
Campos:
    - ID INT AUTO_INCREMENT PRIMARY KEY
    - NOME VARCHAR (150)

2. Tabela "RUBRICAS":
Objetivo: Armazenar as rubricas disponíveis para avaliação de cada um dos parâmetros.
Campos:
    - ID INT AUTO_INCREMENT PRIMARY KEY
    - RUBRICA VARCHAR (2)
    - DESCRICAO VARCHAR (150)
    - VALOR INTEGER
    - CHECK (VALOR_RUBRICA IN (0, 3 ,5))

3. Tabela "PARAM x RUBRICAS":
Objetivo: Armazenar a descrição de todas as rubricas de cada um dos parâmetros da tabela "PARAMETROS".
Campos:
    - ID INT AUTO_INCREMENT PRIMARY KEY
    - NOME_PARAMETRO VARCHAR (150)
    - RUBRICA VARCHAR (2)
    - DESCRICAO_RUBRICA VARCHAR (1000)
    - FOREIGN KEY (NOME_PARAMETRO) REFERENCES parametros(NOME)
    - FOREIGN KEY (RUBRICA) REFERENCES rubricas(RUBRICA)

4. Tabela "REGIOES":
Objetivo: Armazenar as regiões.
Campos: 
    - ID INT PRIMARY KEY
    - NOME_COMPLETO VARCHAR (150)
    - ABREVIACAO VARCHAR (2)

5. Tabela "ESTADOS":
Objetivo: Armazenas os estados.
Campos:
    - ID INT PRIMARY KEY
    - NOME_COMPLETO VARCHAR (150)
    - ABREVIACAO VARCHAR (2)

6. Tabela "REGIOES x ESTADOS":
Objetivo: Armazenar a relação entre as regiões e seus respectivos estados.
Campos:
    - ID INT PRIMARY KEY
    - ABREVIACAO_REGIAO VARCHAR (2)
    - ABREVIACAO_ESTADO VARCHAR (2)
    - FOREIGN KEY (ABREVIACAO_REGIAO) REFERENCES regioes(ABREVIACAO)
    - FOREIGN KEY (ABREVIACAO_ESTADO) REFERENCES estados(ABREVIACAO)

7. Tabela "CIDADES":
Objetivo: Armazenar as cidades.
Campos:
    - ID INT PRIMARY KEY
    - NOME_COMPLETO VARCHAR (150)

8. Tabela "ESTADOS x CIDADES":
Objetivo: Armazenar a relação entre os estados e suas respectivas cidades.
Campos:
    - ID INT PRIMARY KEY
    - ABREVIACAO_ESTADO VARCHAR (2)
    - NOME_CIDADE VARCHAR (150)
    - FOREIGN KEY (ABREVIACAO_ESTADO) REFERENCES estados(ABREVIACAO)
    - FOREIGN KEY (NOME_CIDADE) REFERENCES cidades(NOME_COMPLETO)

9. Tabela "LOJAS":
Objetivo: Amazenar as informações de todas as lojas do grupo.
Campos:
    - ID INT PRIMARY KEY
    - CODIGO_LOJA INT
    - NOME_LOJA VARCHAR (150)
    - NOME_CIDADE VARCHAR (150)
    - ABREVIACAO_ESTADO VARCHAR (2)
    - DATA_CRIACAO DATE
    - DATA_FECHAMENTO DATE
    - FOREIGN KEY (NOME_CIDADE) REFERENCES cidades(NOME_COMPLETO)
    - FOREIGN KEY (ABREVIACAO_ESTADO) REFERENCES estados(ABREVIACAO)


10. Tabela "AVALIACOES":
Objetivo: Armazenar informações gerais de cada uma das avaliações já realizadas.
Campos:
    - ID VARCHAR (300) PRIMARY KEY
    - LOGIN VARCHAR (100)
    - ESTADO VARCHAR (2)
    - CIDADE VARCHAR (150)
    - FILIAL_LOJA INT
    - DATA_AVALIACAO DATE
    - FOREIGN KEY (ESTADO) REFERENCES estados(ABREVIACAO)
    - FOREIGN KEY (CIDADE) REFERENCES cidades (NOME_COMPLETO)
    - FOREIGN KEY (FILIAL_LOJA) REFERENCES lojas(CODIGO_LOJA)
Observação:
    - o canmpo ID deverá ser formado por "estado", "cidade", "filialLoja" e "dataAvaliacao".

11. Tabela "AVALIACAO x PARAMETROS":
Objetivo: Armazenar a avaliação que foi feita para cada um dos parâmetros analisados.
Campos:
    - ID VARCHAR (300) PRIMARY KEY
    - NOME_PARAMETRO VARCHAR (150)
    - AVALIACAO VARCHAR (2)
    - FOREIGN KEY (NOME_PARAMETRO) REFERENCES parametros(NOME)
    - FOREIGN KEY (AVALIACAO) REFERENCES rubricas(RUBRICA)
Observação:
    - o campo ID deverá ser formado por "estado", "cidade", "filialLoja" e "dataAvaliacao".

12. Tabela "LOGIN":
Objetivo: Armazenar as informações de todos os usuários que acessam a plataforma para realizar avaliações.
Campos:
    - ID INT PRIMARY KEY
    - NOME VARCHAR (150)
    - SOBRENOME VARCHAR (150)
    - MATRICULA INT
    - DATA_NASCIMENTO DATE
    - DATA_ADMISSAO DATE
    - DATA_DEMISSAO DATE