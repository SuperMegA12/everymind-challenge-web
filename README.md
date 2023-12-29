# Nunes Sports Web

Este repositório contém o código-fonte do frontend para o desafio EveryMind Challenge, desenvolvido para o cliente fictício Nunes Sports. O frontend foi construído utilizando Vue.js 2, Vuetify 2, Pug e Axios.

## Estrutura do Projeto

O projeto frontend está estruturado da seguinte forma:

- `src/components`: Contém os componentes Vue.js utilizados para criar as páginas e formulários.
  - `CreateProductForm.vue`: Componente para criar novos produtos.
  - `UpdateProductForm.vue`: Componente para atualizar produtos existentes.
  - `ProductList.vue`: Componente para exibir a lista de produtos em uma tabela.
- `src/App.vue`: O componente raiz que organiza as views.


## Funcionalidades Principais

- **Listagem de Produtos:** Visualize todos os produtos disponíveis em uma tabela interativa.
- **Criação de Produtos:** Adicione novos produtos à lista através do formulário de criação.
- **Atualização de Produtos:** Edite informações existentes de produtos usando o formulário de atualização.
- **Exclusão de Produtos:** Remova produtos da lista com confirmação do usuário.

## Personalizações e Melhorias

Além das funcionalidades principais, o projeto possui:

- **Tratamento de Erros:** Mensagens informativas e de erro para feedback do usuário.
- **Melhorias na Experiência do Usuário:** Utilização de diálogos para confirmações e feedback visual.

## Tecnologias Utilizadas

- **Vue.js 2:** Framework JavaScript para construção de interfaces de usuário.
- **Vuetify 2:** Biblioteca de componentes Vue.js baseada no Material Design para um design consistente.
- **Pug:** Linguagem de template para simplificar a criação de markups.
- **Axios:** Cliente HTTP para fazer requisições à API backend.

## Como Executar o Projeto

Siga estas etapas para executar o projeto localmente:

1. Clone o repositório

2. Navegue até o diretório do projeto

3. Instale as dependências:
    ```bash
    npm install
    ```

4. Execute o projeto:
    ```bash
    npm run serve
    ```

O aplicativo estará disponível em [http://localhost:8081](http://localhost:8081).


