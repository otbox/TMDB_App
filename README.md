# TMDB_App
A web project consuming TMDB api for search and list the movies and rate it.


Challenge below:
Criar uma interface de busca onde os usuários possam pesquisar filmes usando a API do
The Movie Database (TMDB): https://developer.themoviedb.org/docs/getting-started.
Stack
● Frontend: React com Typescript
● Backend: Python com Flask
● Banco de Dados: à escolha do candidato
Objetivo
● Exibir os resultados da pesquisa mostrando:
○ Pôster
○ Título
● Permitir clicar em um filme para ver informações detalhadas, como:
○ Elenco
○ Sinopse
○ Data de lançamento
○ Botões de avaliação: o usuário poderá avaliar o filme com uma nota de 1 a 5.
Essa nota poderá ser editada ou deletada.

● Deverá haver uma página "Filmes avaliados", que listará os filmes que o usuários já
avaliou.
Features necessárias
● Página Principal
○ Barra de pesquisa: Busca na API pública do TMDB
○ Listagem de resultados
○ Estados de loading
○ Interação: Ao clicar em um filme, abre um modal/página do filme
● Modal/Página do filme
○ Informações da API pública: sinopse, data de lançamento, lista de elenco
○ Avaliação
■ Se o filme não foi avaliado: o usuário pode dar uma nota
■ Se o filme já foi avaliado: a nota deve ser carregada e o usuário pode
editar ou remover a nota

○ Botão de fechar/voltar para a página principal
● Página "Filmes Avaliados"
○ Listagem dos filmes que o usuário avaliou
○ Além de título/pôster, deve conter a nota do usuário
○ Interação: Ao clicar em um filme, abre um modal/página do filme

O que está sendo avaliado
● Consumo de APIs
● Criação de APIs
● Entendimento de gerenciamento de estado no React
● Tratamento de estados de carregamento (loading) e erros
● Uso adequado de banco de dados

Pontos extras (bônus)
● Paginação ou scroll infinito
● Filtro por gênero ou ano
● Autenticação
● Implementação de cache
● Dockerização da aplicação
Instruções de entrega
● O projeto deve ser entregue de uma das seguintes formas:
○ Link de um repositório público no Github
○ Arquivo compactado (.zip) contendo todo o código-fonte
● A aplicação precisa ser executável em um único comando
● O projeto deve conter um README.md com instruções de como rodar localmente
