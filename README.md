# GoStack 10.0 || Módulo 07

* [1. Conceitos abordados](#1-conceitos-abordados)
* [2. Descrição do projeto](#2-descrição-do-projeto)
* [3. Iniciando o Projeto](#3-iniciando-o-projeto)
* [4. Criando o projeto](#4-criando-o-projeto)

## 1. Conceitos abordados

1.  Redux.
2.  Usando API através de JSON Server e Axios.
3.  Formatação de preço (R$).
4.  Configurção de Redux e integração com React.
5.  Criação de store com: { createStore } from 'redux'.
6.  Reducers (vide 10. Adicionando ao Carrinho):
    1.  **dispatch action, reducers, State**
7.  Reactotron + Redux
8.  mapStatetoProps e mapDispatchToProps.
9.  immer para tratar de produtos duplicados.
10. Refatorando actions.
11. Configurando **Redux Saga**
    1.  Middlewares p/ sideEffects.
    2. Separar actions em 2 Ex. **addToCartRequest e addToCartSuccess**
12. Produto vs. Estoque e mensagens de ERRO via React Toastify.

## 2. Desrição do projeto

Um website 'Rocketshoes', que tem uma lista de produtos (tênis). O site permite adicionar os produtos ao 'Carrinho'. Na página do Carrinho,  podemos alterar a quantidade de cada produto, remover o produto e temos o retorno do valor subtotal e total de produtos em R$.

### Main
<img src="https://github.com/MaisDennis/GoStack10.0-Modulo-07/blob/master/src/assets/Main.png" alt="Main" width="100%" height="auto">

### Carrinho
<img src="https://github.com/MaisDennis/GoStack10.0-Modulo-07/blob/master/src/assets/Cart.png" alt="Cart" width="100%" height="auto">

## 3. Iniciando o projeto

1.  Iniciar a biblioteca JSON Server:
```
json-server server.json -p 3333
```
2.  Iiciar o React
```
yarn start
```
## 4. Criando o projeto

1. Criando o projeto do zero.
   1. Terminal:
      ```
      yarn create react-app modulo05
      ```
      1. webpack, babel se encontram em "react-scripts"
   2. deletar eslintConfig: {}, pois vamos configurar do zero.
   3. ```<div id="root">"``` é onde vai todo o código React.
   4. Delete: comments, manifest, manifest.json, App.css, App.test.js, index.css, logo, serviceWOrker.js

2. ESLint, Prettier & EditorConfig.
   1. Criar modulo05/.editorconfig
   2. Add/Create Eslint (Popular Language: AirBNB)
      ```
      yarn add eslint -D
      yarn eslint --init
      ```
      1. Excluir package-lock.json e yarn.
      ```
      yarn add prettier eslint-config-prettier eslint-plugin-prettier babel-eslint -D
      ```
      2. modulo05/.eslintrc.js
      3. modulo05/.prettierrc

3. Estilos globais
    1. Criar src/styles/global.js (padrão Rocketseat)
        ```
        yarn add styled-components
        ```
    2. Add Roboto
        1. Google -> Roboto -> https://fonts.google.com/specimen/Roboto
        2. Select, @import
    3. Inserir background
        1. import background from "../assets/images/background.svg";
    4. src/App.js: import GlobalStyle from "./styles/global"; Criar componente.

4. Criando Header
    1. Criar src/components/Header/index.js e styles.js
    2. Home/index.js: import logo.svg.
    3. App.js: import Header.
    4. HEader/index.js: adicionar o comp: Link e Cart.
    5. Add Icon to Cart:
        ```
        yarn add react-icons
        ```
    6.Header/styles.js:
        1. Estilizar Container e Cart
            1. Cart = styled(Link)``;
        2. Adicionar opacity e transition.

5. Estilizando a Home
    1. Criar o arquivo src/pages/Home/styles.js
    2. Home/index.js: Trocar Container por ProductList.
    3. Add img, strong, span, button, MdAddShoppingCart.
    4. Copiar o li 5 vezes para ter 5 produtos.
    5. Home/styles.js: estilizar a grid/lista.
        1. button: margin-top: auto (mantem o botão sempre para baixo).
        2. adicionar hover.
            1. Polished lida com cor dentro do JavaScript.
                ```
                yarn add polished
                ```
6. Estilizando o Carrinho
    1. Cart/index.js
    2. Cart/styles.js

7. Configurando API
    1. Usar API fake da Rocketseat (através de JSON server).
        1. Biblioteca JSON Server:
            1. Google -> json-server -> https://github.com/typicode/json-server
            2. Criar json e criar uma API a partir desse arquivo.
                ```
                yarn global add json-server
                ```
            3. arquivo: server.json
            4. Axios.
                ```
                yarn add axios
                ```
            5. Criar src/services/api.js (localhost:3333)
            6. rodar server:
                ```
                json-server server.json -p 3333
                ```
            7. Vide: http://localhost:3333/products

8. Buscando Produtos da API
    1. pages/Home/index.js
        1. import api from '../../services/api';
        2. Transformar function Home => class, criar render:
        3. Criar a função componentDidMount().
        4. Apagar os li a mais, adicionar key= ao li.
        5. Adicionar um products.map dentro do ProductList e li fica dentro do prducts.map.
        6. img: trocar o src, alt, strong, span.
    2. Formatar o preço.
        1. Criar pasta src/util/format.js
        2. Intl é uma função do javascript.
        3. Home/index.js: import { formatPrice } from '../../util/format';
        4. Criar o formatPrice dentro de componentDidMount (assim que chamar a API).

9. Configurando o Redux
    1. Redux e integração Redux com React:
       ```
       yarn add redux react-redux
       ```
    2. Criar src/store/index.js: import { createStore } from 'redux';
    3. src/App.js
        1. Adicionar Provider: Provider vai deixar o store disponivel para todos os compnentes. Provider fica em volta de todos os componentes.
        2. import store from './store'; Provider store={store}
    4. Criar o reducer cart:
        1. Criar store/modules/cart/reducer.js.
    5. Criar rootReducer:
        1. Criar store/modules/rootReducer.js
        2. combine todos os reducers em um unico export.
        3. import rootReducer em store/index.js.

10. Adicionando ao Carrinho (Cart)
    1. Home/index.js: Conectar o component ao state do Redux.
        ```
        import { connect } from 'react-redux';
        ```
        1. Tirar o export default do class e add a ultima linha: export default **connect()(Home);**
    2. Disparar a action:
        1. onClick no button: add função **handleAddProduct**.
        2. **dispatch** serve para disparar uma action (this.props.dispatch).
            1. **type** é obrigatória. product = produto a adicionar.
        3. um dispatch de dentro de um componente do react = **todos os reducers são ativados (ouvem)**.
        4. Criar **switch** para ouvir apenas as actions que interessam ao reducer e ignorar as demais.
            1. case: 'ADD_TO_CART'
            2. **return: adicionar o product ao array: state.**
            3. Todo reducer recebe (state, action).
            4. **State** é o estado antes da action (array vazio).
        5. o **Reducer** avisa todos os **componentes que tem connect** que state foi atualizado.

11. Reactotron + Redux
    1.  ```
        yarn add reactotron-react-js reactotron-redux
        ```
    2.  Criar src/config/ReactotronConfig.js
    3.  .eslintrc.js: eliminar o erro do console.tron.
    4.  Integrando a parte do Redux no Reactotron.
        1.  store/index.js: add enhancer.
    5.  import './config/ReactotronConfig';
    6.  Reactotron:
        1. vide ACTION
        2. Criar State Subscription: Cart

12. Listando no carrinho
    1.  Cart/index.js
        1.  Tirar o export default do class e add a ultima linha: export default **connect(mapStateToProps)(Cart);**
        2.  add mapsStateToProps (snip-it). A partir desse momento, o **Cart() ja tem acesso a { cart }**.
        3.  cortar tr entre tbody e colar entre  {cart.map(product => () )}.
    2.  cart/reducer.js: adicionar produto como objeto com amount: {...action.product, amount: 1,},
    3.  add product.amount

13. Produto Duplicado
    1. Até agora, ao adicionar o produto novamente, ele está sendo listado 2 vezes,
    2. immer: lidar com objects e arrays imutaveis.
        1. Cria Draft e usa ToDos.
        2. cria flexibilidade para alterar states.
        3. Google -> immerjs: https://github.com/immerjs/immer
        ```
        yarn add immer
        ```
        4.  cart/reducer.js
            1. import produce from 'immer';
            2. refazer **ADD** return produce(), incluir if(productIndex == duplicado).
            3. eslint error: incluir regra: 'no-param-reassign': 'off',

14. Remover produto
    1. Cart/index.js: Adicionar o parametro dispath no Cart(), prop onClick no button de delete item.
    2. cart/reducer.js: adicionar o case 'REMOVE_FROM_CART':

15. Refatorando as actions
    1.  Criar 1 arquivo para cada **action**
    2.  criar o arquivo cart/actions.js
        1.  copiar oq ta dentro do dispatch no Home/index.js e criar export function addToCart()
        2.  idem para removeFromCart
    3.  Home/index.js: import * as CartActions from '../../store/modules/cart/actions';
        1.  o * da acesso a CartActions.addToCart e CartActions.removeFromCart
        2.  dispatch(CartActions.addToCart(product));
    4.  import **{ bindActionCreators }** from 'redux';
        1.  nova função **mapDispatchToProps** (com snip-it da Rocketseat)
        2.  converte actions do redux em componentes.
        3.  add ao: export default connect(null, mapDispatchToProps)(Home);
        4.  ```Javascript
            handleAddProduct = product => {
              const { addToCart } = this.props;
              addToCart(product);
            ```
        5.  idem para Cart/index.js
            ...
            ```Javascript
            function Cart({ cart, removeFromCart }) {

            <button type="button">
              <MdDelete
                size={20}
                color="#7159c1"
                onClick={() => removeFromCart(product.id)}
              />
            </button>
            ```
        6.  Refatorando actions deixa elas reutilizavel.
    5.  No Reactotron, é bom saber qual modulo a action está disparando, portanto nomear actions:
        1.  cart/actions.js
            1. type: '@cart/ADD',  type: '@cart/REMOVE',
        2.  cart/reducer.js
            1. case '@cart/ADD': case '@cart/REMOVE':

16. Alterando a Quantidade
    1.  cart/actions.js
        1. Criar export function updateAmount(id, amount)
    2.  Cart/index.js
        1.  Criar functions (+) increment(product) e (-) decrement(product).
    3.  cart/reducer.js
        1.   case '@cart/UPDATE_AMOUNT':
    4.  obs. o reducer é responsavel por todas as regras de utilidade (ex. amount < 0). Não é responsabilidade da interface.

17. Calculando totais
    1.  Os cálculos, não ocorrerão no reducer, nem no render, e sim no **mapStateToProps**.
    2.  Cart/index.js
          1.  import { formatPrice } from '../../util/format';
          2.  mapStateToProps: o cart pode ser retornado do jeito que quiser. cart: state.cart.map()
          3.  Calculando subtotal:
              1.  Adicionar ao strong com subtotal.
              2.  Calculo só vai executar quando alguma informação do reducer atualizar.
          4. Calculando Total:
              1.  mapsStateToProps: total: state.cart.reduce((total, product).
                  1. **reduce**: 1 unico valor para todo array.
              2.  total: formatPrice(...)

18. Exibindo quantidades
    1.  Home/index.js
        1.  Criar um **mapStateToProps**, return amount.
        2.  Add amount ao carrinho. MdAddShoppingCart: {amount[product.id] || '-'}

19. Configurando Redux **Saga** (Segue o roteiro abaixo, não tem jeito :/ )
    1.  Sagas: Middlewares dentro do Redux. (interceptors p/actions).
        1.  toda vez que disparar uma action, um middleware pode ser acionada para fazer algum efeito colateral = sideEffect.
        2.  Vamos buscar otras infos do produto ao adicionar ao carrinho.
        3.  ```
            yarn add redux-saga
            ```
    2.  Criar modules/cart/sagas.js
        1.  * = generator (como se fosse uma async)
            ```
            function* addToCart(action) {}
            ```
        2. Trocar o parametro addToCart(product -> id) em actions.js
        3.  Home/index.js: puxar somente o id.
            ```Javascript
            <button
              type="button"
              onClick={() => this.handleAddProduct(product.id)}
            >

            handleAddProduct = id => {
              const { addToCart } = this.props;
              addToCart(id);
            ```
        4.  sagas.js: import **call**
            ```Javascript
            import { call } from 'redux-saga/effects';
            import api from '../../../services/api';
            function* addToCart({ id }) {
              const response = yield call(api.get, `/products/${id}`);
            }
            ```
        5.  actions.js
            1. Separar addToCart em **addToCartRequest e addToCartSuccess**
                1.  obs. request é ouvido apenas pelo saga (e não pelo reducer).
                2.  saga finaliza a chamada API e tem dados do produto, chama a Success.
                3.  Success é recebida pelo reducer (É como um passo a mais).
        6.  cart/reducer.js
            1. alterar: case '@cart/ADD_SUCCESS':
        7.  Home/index.js:  alterar const { addToCartRequest } = this.props;
        8.  sagas.js:
            1.  import **put**. put dispara uma action no redux
            2.  import { addToCartSuccess } from './actions';
            3.  yield put(addToCartSuccess(response.data));
            4.  ```Javascript
                import { call, put, all, takeLatest } from 'redux-saga/effects';

                export default all([takeLatest('@cart/ADD_REQUEST', addToCart)]);
                ```
                1.  export default all...cadastrar varios linsteners.
                2.  takeLatest = ultimo click, takeEvery = todos os clicks rapidos serão considerados.
                3.  takeLatest -> 2 parametros: action a ouvir, action a disparar.
    3.  Configurar o saga dentro do redux
        1.  Criar modules/rootSaga.js
            1.  vai juntar todas as Sagas em 1 arquivo (como rootReducer).
        2.  atualizar store/index.js

20.  Reactotron + Saga
    1.  plugins no Reactotron.
        ```
        yarn add reactotron-redux-saga
        ```
    2.  config/ReactotronConfig.js: import reactotronSaga from 'reactotron-redux-saga';
    3.  store/index.js: criar sagaMonitor

21. Separando actions
    1.  cart/reducer.js: alterar ADD_SUCCUESS
        1. apenas: draft.push(product); o resto acontece no sagas.
    2.  cart/sagas.js:
        1.  adicionar const data com as informações de reducer.
        2.  Não duplicar o produto:
            1.  import {  select } from 'redux-saga/effects'; buscar informações dentro do state.
            2.  const productExists = yield select...

22. Estoque na adição
    1.  cart/sagas.js:
        1.  Criar const stock
        2.  ...
            ```Javascript
            if (amount > stockAmount) {
              console.tron.warn('ERRO');
              return; }
            ```

23. React Toastify
    1.  Mensagem de ERRO. p/ front user.
    2.  src/App.js: import { ToastContainer } from 'react-toastify'; adicionar ToastContainer />
    3.  src/styles/global.js: import 'react-toastify/dist/ReactToastify.css';
    4.  modules/cart/sagas.js: add toast.error('Quantidade solicitada fora de estoque');

24. Estoque na alteração ( quantidade)
    1.  modules/cart/actions.js
        1.  separar updateAmount em 2 (sempre que for trabalhar com Saga)
            1.  updateAmountRequest(id, amount)
            2.  updateAmountSuccess(id, amount)
    2.  pages/Cart/index.js
        1.  Alterar occurences: updateAmount para updateAmountRequest.
    3.  modules/cart/sagas.js
        1.  Alterar all occurences: updateAmount para updateAmountSuccess.

25. Navegação de página dentro do Saga **(Funcionalidade excluída do projeto)**
    1.  Ao clicar em "Adicionar ao Carrinho", ir direto a pagina /Cart.
    2.  Poŕem, a navegação acontece depois de um dispatch do Saga, portanto vamos ter que fazer a navegação via Saga. Obs. javascript nao sabe que o saga esta sendo executado, portanto pode acontecer a mudança de pagina antes da adição ao carrinho.

    3.  ```
        yarn add history
        ```
    4.  Criar services/history.js
    5.  src/App.js:
        1.  import history from './services/history'; import { Router } from 'react-router-dom';
        2.  Router history={history}>
    6.  cart/sagas.js:  incluir history.push
    7.  teste: adicionar ao carrinho direciona a pagina do carrinho.
        1.  colocar um delay no carregamento, Terminal:
        ```
        json-server server.json -p 3333 -d 2000
        ```



