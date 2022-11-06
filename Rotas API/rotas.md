O objetivo desse documento é descrever todas as informações relacionadas às rotas da API que deverão ser criadas para essa aplicação.

As rotas foram modeladas para satisfazer o modelo CRUD:
    - Criar;
    - Ler;
    - Atualizar;
    - Deletar.

Segue abaixo descrição e requisitos para criação de cada uma das rotas. Para melhor compreensão e organização, foram separadas conforme operações do modelo CRUD:

1. GET:

- '/listarParametros':
    - busca na tabela "PARAMETROS";
    - retorna todos os itens do campo "NOME".

- '/listarRegioes':
    - busca na tabela "REGIOES";
    - retorna todos os itens do campo "ABREVIACAO".

- '/listarEstados':
    - busca na tabela "ESTADOS";
    - retorna todos os itens do campo "ABREVIACAO".

- '/listarEstados/:regiao':
    - busca na tabela "REGIOES x ESTADOS";
    - recebe como parâmetro a regiao indicada em ":regiao" e filtra pelo campo "ABREVIACAO_REGIAO";
    - retorna todos os itens do campo "ABREVIACAO_ESTADO".

- '/listarCidades/:estado/':
    - busca na tabela "ESTADOS x CIDADES";
    - recebe como parâmetro o estado indicado em ":estado" e filtra pelo campo "ABREVIACAO_ESTADO";
    - retorna todos os itens do campo "NOME_CIDADE".

- '/listarLojas/:estado/:cidade':
    - busca na tabela "LOJAS";
    - recebe como parâmetro o estado indicado em ":estado" e filtra pelo campo "ABREVIACAO_ESTADO";
    - recebe como parâmetro a cidade indicado em ":cidade" e filtra pelo campo "NOME_CIDADE";
    - retorna todos os itens do campo "CODIGO_LOJA".

- '/buscarAvaliacao/:id':
    - busca na tabela "AVALIACAO x PARAMETROS";
    - recebe como parâmetro o ID que é composto pelos campos "estado", "cidade", "filialLoja" e "dataAvaliacao" e filtra pelo campo "ID" da tabela;
    - retorna um array de objetos com cada um dos parâmetros e suas respectivas notas:<br>
        [<br>
            {<br>
                "parametro": nomeParametro,<br>
                "nota": notaParametro<br>
            },<br>
            {<br>
                "parametro": nomeParametro,<br>
                "nota": notaParametro<br>
            },<br>
            [...]<br>
        ]<br>

- '/buscarAvaliacoesPorRegiao/:parametro/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES", "REGIOES x ESTADOS" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - retorna o valor médio, por região, do parâmetro indicado em ":parametro".

- '/buscarAvaliacoesPorEstados/:parametro/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES", "ESTADOS" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - retorna o valor médio, por estado, do parâmetro indicado em ":parametro".

- '/buscarAvaliacoesPorEstadosDeRegiao/:regiao/:parametro/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES", "REGIOES x ESTADOS" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - retorna o valor médio, por estado da região indicada em ":regiao", do parâmetro indicado em ":parametro".

- '/buscarAvaliacoesPorCidadeDoEstado/:estado/:parametro/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - recebe como parâmetro no body se deve retornar o valor médio do parâmetro de todas as cidades ou apenas das "top10";
    - retorna o valor médio, por cidade do estado indicado em ":estado", do parâmetro indicado em ":parametro".

- '/buscarAvaliacoesPorLojaDaCidade/:estado/:cidade/:parametro'/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - recebe como parâmetro no body se deve retornar o valor médio do parâmetro de todas as lojas da cidade ou apenas das "top10";
    - retorna o valor médio, por loja da cidade indicada em ":cidade" do estado indicado em ":estado", do parâmetro indicado em ":parametro".

- '/buscarAvaliacoesDaLoja/:filial_loja/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - retorna as avaliações de cada um dos parâmetros avaliados na loja indicada em ":filial_loja".

- '/buscarAvaliacoesDaLojaPorParametro/:parametro/:filial_loja/?dataInicial="dd/mm/yyyy"&dataFinal="dd/mm/yyyy':
    - busca na junção das tabelas "AVALIACOES" e "AVALIACAO x PARAMETROS";
    - definir o período das avaliações a partir dos parâmetros passados em "dataInicial" e "dataFinal" do endpoint;
    - retorna as avaliações do parâmetro indicado em ":parametro" da loja indicada em ":filial_loja".


2. POST:

- '/criarAvaliacao/'
    - recebe como parâmetro 1 objeto (body1):
    - body1:<br>
    {<br>
        "login": loginUsuario,<br>
        "estado": estado,<br>
        "cidade": cidade,<br>
        "filialLoja": filialLoja,<br>
        "dataAvaliacao": dataAvaliacao<br>
    }<br>
    - recebe como parâmetro um array de objetos (body2):<br>
    [<br>
        {<br>
            "parametro": limpeza,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": vitrine,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": iluminacao,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": estoque,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": engajamentoTime,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": combateIncendio,<br>
            "nota": nota<br>
        },<br>
    ]<br>
    - com o body1, deverá ser criado registro na tabela "AVALIACOES";
    - com o body1 e body2, deverá ser criado 1 registro na tabela "AVALIACAO x PARAMETROS" para cada objeto do array do body2, sendo que o ID deverá ser definido pelos campos "estado", "cidade", "filialLoja" e "dataAvaliacao" do body1.


3. PUT:

- '/atualizar/:id':
    - atualiza dados da tabela "AVALIACAO x PARAMETROS";
    - recebe como parâmetro o ID que é composto pelos campos "estado", "cidade", "filialLoja" e "dataAvaliacao" e filtra pelo campo "ID" da tabela;
	- DÚVIDA: devo mencionar que as infos "estado", "cidade", "filialLoja" e "dataAvaliacao" são obtidas através de uma função do arquivo .js que pega as infos do DOM e, então, gera o ID que é passado em ":id"?
    - recebe via body um array de objetos que contém os valores atualizados de cada parâmetro:<br>
    [<br>
        {<br>
            "parametro": limpeza,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": vitrine,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": iluminacao,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": estoque,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": engajamentoTime,<br>
            "nota": nota<br>
        },<br>
        {<br>
            "parametro": combateIncendio,<br>
            "nota": nota<br>
        },<br>
    ]<br>

4. DELETE:
- '/deletar/:id':
    - deleta registro da tabela "AVALIACOES";
    - deleta registros da tabela "AVALIACAO X PARAMETROS";
    - recebe como parâmetro 1 objeto (body1):<br>
    {<br>
        "estado": estado,<br>
        "cidade": cidade,<br>
        "filialLoja": filialLoja,<br>
        "dataAvaliacao": dataAvaliacao<br>
    }<br>
    - recebe como parâmetro o ID que é composto pelos campos "estado", "cidade", "filialLoja" e "dataAvaliacao" e filtra pelo campo "ID" de cada uma das tabelas.