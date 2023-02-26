# ReactJs




## React 事件机制
React并不是将click事件绑定到了div的真实DOM上，而是在document处监听了所有的事件
> React ne lie pas l'évélement au vrai DOM, juste ajouter un lisenner au niveau du document


## Hooks是什么
它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。通过自定义hook，可以复用代码逻辑。
> On peut utiliser state ou autre caractéristiques de React sans coder en class

## React 高阶组件是什么，和普通组件有什么区别，适用什么场景
HOC(Higher-Order Components) 自身不是 React API, 它是一种基于 React 的组合特性而形成的设计模式。
例如页面授权，某些页面需要权限，我们可以创建一个高阶组件来进行判断。
> C'est pas une React API, il est en base d'une caracteristique de React, c'est une Design pattern
> Par exemple, autorisation de la page, on peut ecrire un HOC qui retourne le page si autorisé


## React如何获取组件对应的DOM元素？
可以用ref来获取某个子节点的实例， 函数式或者使用useRef()。
> utiliser ref pour obtenir une instance d'un nœud, soit par fonction, soit par useRef().

## React中可以在render访问refs吗？为什么？
不可以，render 阶段 DOM 还没有生成，无法获取 DOM。
> non on peut pas, parce que DOM est pas encore généré dans l'étape de rendu, donc on ne peut pas le récupéré

## 在React中如何避免不必要的render？
使用 React.memo, 用来缓存组件的渲染，避免不必要的更新
> utiliser React.memo, qui va mettre en cache le rendu des composants pour éviter les mises à jour inutiles

## 对 React context 的理解
在React中，数据传递一般使用props传递数据，维持单向数据流。 当需要传递的数据需要跨越多层组件时，我们可以用Context。可以把context当做是特定一个组件树内共享的store
> En générale, transfere les données à l'aide de props. S'il doit passer plusieur composant, props va etre compliqué. Et donc c'est mieux on utilise Context. Ca peut etre considéré comme un store qui partage dans ce arbre des composants

## React中什么是受控组件和非控组件？
**受控组件** 例如`<input><select><textearea>`等元素，我们可以通过绑定事件来获取他相对应的值。
> **Composants contrôlés** lier à des événements pour obtenir leurs valeurs correspondantes.

**非受控组件** 没有对应的value 或 props。需要依赖ref或别的方法来获取值。
> **Composants non contrôlés** Il n'y a pas de valeurs ou props. Besoin utilise ref ou autre methode pour obtenir les valeurs

# 数据管理

## React setState 原理
在调用setState之后将新的state加入一个队列，并于当前状态进行比对。然后重新渲染整个UI界面。React会自动算出差异的地方，然后进行部分更新。

## setState 是同步还是异步的
绝大多数情况时异步的。在某些情况例如addEventListener， setTimeout， setInterval等事件中是同步的。

## setState的第二个参数作用是什么？
是一个回调，可以拿到更新后的state值。
> c'est un callback qui reçoit la nouveau state apres la mise à jour.

## state 是怎么注入到组件的，从 reducer 到组件经历了什么样的过程
通过`connect`和`mapStateToProps mapDispatchToProps`将state注入到组件中。
reducer对action对象处理，更新组件状态，并将新的状态值返回store。
> reducer traite l'objet action, met à jour l'état du composant et renvoie la nouvelle valeur au store.

## React组件的state和props有什么区别？
props是一个从外部传进组件的参数，主要作为就是从父组件向子组件传递数据，它具有可读性和不变性
> props est un paramètre passé dans le composant depuis l'extérieur. Il est lisible et invariable

state的主要作用是用于组件保存、控制以及修改自己的状态
> state permet aux composants de sauvgarder, controler et modifier son propre etat.

## React中的props为什么是只读的？
避免产生副作用
> Éviter les effets secondaires


# 组件通信
React组件间通信常见的几种情况:

- 父组件向子组件通信
- 子组件向父组件通信
- 跨级组件通信
- 非嵌套关系的组件通信


## 父组件向子组件通信
父组件通过 props 向子组件传递需要的信息。
> Le composant parent transmet les informations requises au composant enfant via les props.

## 子组件向父组件通信
props+回调的方式。
> props + callbacks.

## 跨级组件通信
- 使用props，利用中间组件层层传递
- 使用context，context相当于一个大容器，可以把要通信的内容放在这个容器中



## 非嵌套关系的组件通信
- 可以使用自定义事件通信（发布订阅模式pubsub）
    > publish-subscribe
- 可以通过redux等进行全局状态管理



# 路由
## react-router实现
基于 history 库来实现上述不同的客户端路由实现思想


```jsx
<Routes>
    <Route path="/" element={<Products/>}>
</Routes>
```
## react-router 里的 Link 标签和 a 标签的区别
这两者都是链接， 但`<Link>`的跳转行为只会触发相匹配的`<Route>`对应的页面内容更新，而不会刷新整个页面。

## React-Router的路由有几种模式？
React-Router 支持使用 hash（对应 HashRouter）和 browser（对应 BrowserRouter） 两种路由规则


# Redux
Redux 提供了一个叫 store 的统一仓储库，组件通过 dispatch 将 state 直接传入store，不用通过其他的组件。并且组件通过 subscribe 从 store获取到 state 的改变。

将代码分开，便于维护

## Redux 怎么实现属性传递，介绍下原理
react-redux 数据传输∶ view-->action-->reducer-->store-->view。

## Redux 中间件是怎么拿到store 和 action? 然后怎么处理?
middleware 的函数签名是（{ getState，dispatch })=> next => action。

## Redux中的connect有什么作用
connect负责连接React和Redux

connect 通过 context获取 Provider 中的 store，通过 store.getState() 获取整个store tree 上所有state
> connect obtenir le store via le context dans un balise Provider


# Hooks
## React Hook 的使用限制有哪些？

- 不要在循环、条件或嵌套函数中调用 Hook；
- 在 React 的函数组件中调用 Hook。

## useEffect 与 useLayoutEffect 的区别

首先useLayoutEffect总是比useEffect先执行。

useLayoutEffect会在所有DOM变更后 *同步执行*

UseEffect有时候会使屏幕内容闪烁，而useLayoutEffect不会

> useLayoutEffect est toujours executé avant que useEffect


# React Hooks 和生命周期的关系？

函数组件 的本质是函数，没有 state 的概念的，因此不存在生命周期一说，仅仅是一个 render 函数而已。

但是引入 Hooks 之后就变得不同了，它能让组件在不使用 class 的情况下拥有 state，所以就有了生命周期的概念，所谓的生命周期其实就是 useState、 useEffect() 和 useLayoutEffect() 。



# Real DOM和Virtual DOM

## 区分Real DOM和Virtual DOM
__React DOM__: 更新慢，可以直接更新html，元素更新创建新DOM，DOM操作代价高
> Il peut mettre à jour le html directement, 
> les mises à jour des éléments qui va créer un nouveau DOM,
> par contre il est plus lent que Virtual Dom

__Virtual DOM__: 更新快，不可直接更新html，元素更新则更新JSX，DOM操作简单
> Il peut pas mettre à jour le html,
> et chaque fois on mise à jour des élement qui manipule en fait l'élément jsx


## React diff 算法的原理是什么？
diff 算法探讨的就是虚拟 DOM 树发生变化后，生成 DOM 树更新补丁的方式。它通过对比新旧两株虚拟 DOM 树的变更差异，将更新补丁作用于真实 DOM，以最小成本完成视图更新。



## React key 是干嘛用的 为什么要加？key 主要是解决哪一类问题的

Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。

在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系。


## React 数据持久化有什么实践吗？

```jsx
let storage={
    // 增加
    set(key, value){
        localStorage.setItem(key, JSON.stringify(value));
    },
    // 获取
    get(key){
        return JSON.parse(localStorage.getItem(key));
    },
    // 删除
    remove(key){
        localStorage.removeItem(key);
    }
};
export default Storage;

```


在已经使用redux来管理和存储全局数据的基础上，可以用redux-persist。redux-persist会将redux的store中的数据缓存到浏览器的localStorage中。


## React 设计思路，它的理念是什么？
- 编写简单直观的代码
- 简化可复用的组件
- Virtual DOM
- 函数式编程
- 一次学习，随处编写




