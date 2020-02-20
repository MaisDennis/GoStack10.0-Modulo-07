### Conceitos abordados:
1. Iniciar um projeto React Native para mobile app.
2. Utilização de ambiente Android.
3. Incluir ESLint, Prettier & EditorConfig.
4. debug via Reactotron.
5. Navegação entre páginas: React Navigation.
6. Estilização via Styled Components.
7. Acessando dados de API do Github.
8. Acessando localstorage do Google Chrome.
___

### Desrição do projeto:

### Iniciando o projeto:

1. yarn start
2. json-server server.json -p 3333

### Criando o projeto:

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

12. Cart/index.js
    1.  Tirar o export default do class e add a ultima linha: export default **connect(mapStateToProps)(Cart);**
    2.  add mapsStateToProps (snip-it). A partir desse momento, o Cart() ja tem acesso a { cart }.
    3.  cortar tr entre tbody e colar entre  {cart.map(product => () )}.
    4.  Header/index.js: Add Link to="/cart" em volta do Cart.
