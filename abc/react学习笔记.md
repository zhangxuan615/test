# 1 JS解构赋值与扩展运算符

## 1.1 数组的解构

### 1.1.1 传统做法

利用下标引用数组中的元素

```react
var array = [1, 'one', true];
//控制台输出1
console.log(array[0]);
```

### 1.1.2 ES6做法

```react
// 例1
var [x, y] = [5, 'a'];
console.log(x);				//5
console.log(y);				//a

// 例2：可以省略部分变量
var [, , c] = [1, 2, 3];    //c 被赋值为 3
```

## 1.2 对象的解构

### 1.2.1 ES6做法

```react
var object = {
id : '01',
name : 'hong'
};
// id name 会被赋值为01 hong
var {id, name} = object;

// 也可以省略某些变量
var {id} = object;

// 使用别名
let {id:personId} = object;

// 如果定义的变量不存在对象中，会被赋值为undifined
// 可以默认值，让没有定义的变量也不会为未定义
// id 会被赋值为 01
// age会被赋值为undifined
// name会被赋值为'zx'
var {id, age, name = 'zx'} = object;

```

## 1.3 ES6扩展运算符...[]、...{}用法

> **扩展运算符(…)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中**，
>
> **意味着数组或对象中元素的拷贝只拷贝在栈中的值（对象引用），浅拷贝**

### 1.3.1 ...[]

用法：...将数组序列化，成为逗号隔开的序列

1. **用于解构一个数组之后再重组一个数组，即复制数组**

   ```
   const n = [...[1, 2, 3]];
   // 复制arr1数组，产生的arr2与arr1是两个独立的数组
   const arr1 = [1, 2];
   const arr2 = [...arr1];
   console.log(arr1 === arr2)   // false
   ```

   

2. **将数组转换为参数序列，用于函数参数**

   ```
   console.log(...[1, 2, 3])    // 控制台打印 1 2 3
   
   function add(x, y) {
     return x + y;
   }
   const numbers = [4, 38];
   add(...numbers)  			// 42
   ```

3. **扩展运算符与解构赋值结合起来用于生成数组**

   ```
   const [first, ...rest] = [1, 2, 3, 4, 5];
   first // 1
   rest  // [2, 3, 4, 5]
   
   // 扩展运算符还可以将字符串转为真正的数组
   [...'hello'] 			// [ "h", "e", "l", "l", "o" ]
   
   ```

   需要注意的一点是：

   > **如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。**

### 1.3.2  ...{}

1. **用于解构一个对象之后再重组一个对象**

   ```
   // 用于复制对象
   {...{a: 10, b: 11}, c: 12}   // 最终获得的对象为{a: 10, b: 11, c: 12}
   
   // 同名覆盖
   let bar = {a: 1, b: 2};
   let baz = {...bar, ...{a:2, b: 4}};  // {a: 2, b: 4}
   
   // 对象内部元素是浅拷贝
   let obj1 = { a: 1, b: 2, c: {nickName: 'd'}};
   let obj2 = { ...obj1};
   obj2.c.nickName = 'd-edited';
   console.log(obj1); // {a: 1, b: 2, c: {nickName: 'd-edited'}}
   console.log(obj2); // {a: 1, b: 2, c: {nickName: 'd-edited'}}
   ```

   > 需要注意的是：扩展运算符对对象实例的拷贝属于一种浅拷贝。
   >
   > javascript中有两种数据类型：基础数据类型、引用数据类型
   >
   > 基础数据类型：按值访问，常见的基础数据类型有Number、String、Boolean、Null、Undefined，这类变量的拷贝的时候会完整的复制一份；
   >
   > 引用数据类型：如Array，拷贝的是对象的引用，当原对象发生变化的时候，拷贝对象也跟着变化

2. **扩展运算符与解构赋值结合起来用于生成对象**

   ```
   const {a, ...rest} = {a: 1, b: 2, c: 3, d: 4,};
   a;  // 1
   rest; // {b: 2, c: 3, d: 4,};
   ```

   同数组一样，需要注意的一点是：

   > **如果将扩展运算符用于对象赋值，只能放在参数的最后一位，否则会报错。**



# 2. 函数

## 2.1 函数的定义

```
// 常规定义
function f() {
	console.log('hello world');
}

// 函数对象定义
// 1.
const func1 = function f() {
	console.log('hello world');
}
// 2.
const func2 = function () {
	console.log('hello world');
}
```

## 2.2 call()、apply()、bind()的区别及用法

### 2.2.1 call()与apply()用法比较

> - **两者的第一个参数为this要指向的对象，当第一个参数为null、undefined的时候，默认指向window；**
> - ***均会执行***
> - **apply()后面传入参数可以只能是数组**
> - **call()后面传入的是参数列表，参数可以是任意类型**

```
// 例：
var obj = {a: 101}		//定义一个空的对象
function f(x,y){
    console.log(x,y)
    console.log(this)	//this是指obj
}
f.apply(obj,[1,2]) 		//后面的值需要用[]括起来
f.call(obj,1,2) 		//直接写
// 二者均输出 1 2 {a: 101}
```

### 2.2.2 call()与bind()用法比较

> - **两者的第一个参数为this要指向的对象，当第一个参数为null、undefined的时候，默认指向window；**
> - **call()改过this的指向后，会再执行函数**
> - **bind()改过this后，不执行函数，会返回一个绑定新this的函数**

```
// 例：
var obj = {a: 101}		//定义一个空的对象
function f(x,y){
    console.log(x,y)
    console.log(this)	//this是指obj
}
const g = f.bind(obj, 1);		// 此时g相当于f(1)，不过this指向obj对象
g(10);                          // 此时的10传递给f的第二个参数y
// 输出：1 10 {a:101}
```

# 3 redux源码解读

```
connect(
    mapStateToProps,
    mapsDispatchToProps,
)(App)
ps.
a. mapStateToProps 形式：只能是一个返回对象的函数，返回值被解构为子组件属性 props
const mapStateToProps = (state) => {   
	// state 为 store.getState() 中维护的状态
    return {
        comments: state.comments
    }
}
b. mapsDispatchToProps 形式：函数、对象
函数：对象为函数，直接调用
	只能是一个返回对象的函数，直接调用返回值被解构为子组件属性 props
对象：actionCreators -- 调用 bindActionCreators(actionCreators, dispatch)
	1.对象内的每个属性都是一个函数，该函数为 actionCreator(data) 函数
	2.通过调用 bindActionCreators(actionCreators, dispatch) 返回值被
	  解构为子组件属性
```

# 以下是详细的分析

## 3.1 bindActionCreator(actionCreator, dispatch) 函数

> 参数：
>
> 1、
>
> ​	a. actionCreator 为函数时，返回值同样为一个函数( 在 redux 中不会用到此种情况 )
>
> ​	b. actionCreator 为对象时，返回值同样为一个对象( 在 redux 中使用此种情况 )
>
> 2、dispatch函数

```
function bindActionCreator(actionCreator, dispatch) {
  // 这个函数的主要作用就是返回一个函数，当我们调用返回的这个函数的时候，就会自动的	 dispatch对应的action
  
  // 多种返回形式，
}
```

1. **通过apply()函数实现**

   ```javascript
     // 形参(...args)通过解构获取数组
     // apply第二个参数为数组形式
     return function(...args) {
     		return dispatch(actionCreator.apply(this, args))
     }
   
     // js函数内部维护了一个函数参数数组：arguments
     return function() { 
     		return dispatch(actionCreator.apply(this, arguments)) 
     }
   ```

2. **通过箭头函数实现**

   ```
   // 返回箭头函数
   // 参数中(...args)解构构造
   // 函数体中(...args)通过扩展运算符解析
   return (...args) => dispatch(actionCreator(...args))
   ```

## 3.2 bindActionCreators(actionCreator, dispatch) 函数

> 总结：
> boundActionCreators主要是用来将actions转化为dispatch(action)这种格式，方便进行actions的分离，并且使代码更简洁
>
> 源码中主要是用到了将传入对象的所有 key 值进行遍历，转化为一个函数抛出一个对象的形式，这个抛出的对象中，key 值是传入对象的 key 值，value是一个函数内部执行dispatch(action)，将传入对象的value返回值当做action传了进去。

```
/**
* 参数说明： 
* actionCreators: actionCreate 函数，
*				  可以是一个单函数（在 redux 中不会使用这种情况），
*				  也可以是一个对象-对象的所有元素都是 actionCreate() 函数
* dispatch: store.dispatch方法
*/
export default function bindActionCreators(actionCreators, dispatch) {

  // actionCreators是单函数，此种情况在 redux 中不会被使用
  // 为函数中，该函数会被直接调用，而不会使用 bindActionCreators 这个函数
  if (typeof actionCreators === 'function') {
    return bindActionCreator(actionCreators, dispatch)
  }
  
  // actionCreators 必须是函数或者对象中的一种，且不能是null
  if (typeof actionCreators !== 'object' || actionCreators === null) {
    throw new Error(
      `bindActionCreators expected an object or a function, instead received ${actionCreators === null ? 'null' : typeof actionCreators}. ` +
      `Did you write "import ActionCreators from" instead of "import * as ActionCreators from"?`
    )
  }

  // actionCreators是对象
  // 获取所有actionCreate函数的名字
  const keys = Object.keys(actionCreators)
  // 保存dispatch和actionCreate函数进行绑定之后的集合
  const boundActionCreators = {}
  for (let i = 0; i < keys.length; i++) {
    const key = keys[i]
    boundActionCreators[key] = bindActionCreator(actionCreators[key], dispatch)
  }
  return boundActionCreators
  
  // 另一种写法
  const keys = Object.keys(actionCreators)
  const boundActionCreators = {}
  keys.forEach((key) => {
      boundActionCreators[key] = bindActionCreator(actionCreators[key], dispatch)
  })
  return boundActionCreators
   
  // 绑定之后的对象
  /**
  *  boundActionCreators的基本形式就是
  *  {
  *    actionCreator: function() {dispatch(actionCreator.apply(this, arguments))}
  *  }
  */
}
```

## 3.3 mapStateToProps(state, ownProps)

> - `mapStateToProps` 是一个函数，用于建立组件跟 `store` 的 `state` 的映射关系

```
const mapStateToProps = (state) => {
    return {
        comments: state.comments
    }
}
```

## 3.4 mapDispatchToProps(dispatch)

> - `mapDispatchToProps` 用于建立组件跟 `store.dispatch` 的映射关系
> - 可以是一个`object`，也可以传入函数

### 3.4.1 mapDispatchToProps是一个函数

```
// 必须返回一个对象
const mapDispatchToProps = (dispatch) => {
    return {
        resetComments: (comments) => {
            dispatch(resetComments(comments))
        },
        addComment: (comment) => {
            dispatch(addComment(comment))
        },
        deleteComment: (comment) => {
            dispatch(deleteComment(comment))
        },
    }
}
```

### 3.4.2 mapDispatchToProps是一个对象

```
{
	resetComments, addComment, deleteComment,
}

// 等价于

bindActionCreators({
	resetComments, addComment, deleteComment,
}, dispatch);

// 等价于

{
    resetComments: (comments) => {
    	dispatch(resetComments(comments))
    },
    addComment: (comment) => {
    	dispatch(addComment(comment))
    },
    deleteComment: (comment) => {
    	dispatch(deleteComment(comment))
    },
}
```

## 3.5 connect源码（自己写的，逻辑肯定没那么严谨，未考虑为对象时的情况）

> - **mapStateToProps、mapDispatchToProps这两个函数重点关注其返回值**
> - **返回值一定都是一个对象，然后将对象利用扩展运算符解包，传递作为子组件的属性props**

```react
export const connect = (mapStateToProps, mapDispatchToProps) => (WrappedComponent) => {
    class Connect extends Component {

        static contextTypes = {
            store: PropTypes.object
        }

        constructor() {
            super()
            this.state = {
                allProps: {}
            }
        }

        componentWillMount() {
            const { store } = this.context
            this._updateProps()
            store.subscribe(() => this._updateProps())
        }

		// 只是大概的意思，状态state改变，则从这里开始更新组件树
        _updateProps() {
            const { store } = this.context
            let stateProps = mapStateToProps
                ? mapStateToProps(store.getState(), this.props)
                : {} // 防止 mapStateToProps 没有传入
            let dispatchProps = mapDispatchToProps
                ? mapDispatchToProps(store.dispatch, this.props)
                : {} // 防止 mapDispatchToProps 没有传入
            this.setState({
                allProps: {
                    ...stateProps,
                    ...dispatchProps,
                    ...this.props,
                }
            })
        }

        render() {
            return <WrappedComponent {...this.state.allProps} />
        }
    }
    return Connect
}
```

# 4. React 文件目录

components -- UI组件：不使用 redux、react-redux 的api，只负责 UI 呈现

​											一般保存在 components 文件夹下，例如：App

containers -- 容器组件：负责管理数据和业务逻辑，不负责 UI 呈现

​											一般保存在 containers 文件夹下，例如：connect(x, y)(App)

redux/react-redux -- 状态管理文件夹：store、reducer、actionCreators

UI 组件  → connect（负责整合出完整功能的组件） →  容器组件

异步处理：redux 不支持异步操作，延迟之后通过同步来执行

> 异步处理：redux 默认不支持异步操作，但是 actionCreator() 能够返回函数，延迟之后再调用同步执行

```react
// 异步action 增加
export const incrementAsync = (number) => (
    dispatch => {
        // 异步代码
        setTimeout(() => {
            // 1s之后采取分发action
            dispatch(increment(number))
        }, 1000)
    }
)
// 异步返回函数的另外一种写法
export const decrementAsync = (number) => {
    return dispatch => {
        // 异步代码
        setTimeout(() => {
            // 1s之后采取分发action
            dispatch(decrement(number))
        }, 1000)
    }
}
```

# 5. React/JavaScript 拾遗

## **5.1 父子组件状态同时发生变化的执行顺序**

​    父子组件状态同时发生变化，只执行父组件生命周期函数，因为执行父组件生命周期函数意味着一定会执行子组件对应的生命周期函数，即不需要再单独去执行一边子组件的生命周期函数了

## **5.2 constructor 中 this.state = {}设置的状态会重置 state = {} 中设置的状态**

​    先执行 state = {}，再去执行 constructor(props) 初始化函数

## **5.3 在同一个状态周期先后设置状态，后设置的状态会覆盖先设置的状态（状态在被不断更新）**

## **5.4 生命周期函数中触发状态设置**

​	5.4.1 不会触发额外渲染

> componentWillMount()、componentWillReceiveProps(nextProps)

​	5.4.2 会触发额外渲染

> render()、componentDidMount(): 必须等待本轮的生命周期执行完成（包括父组件）
>
> shouldComponentUpdate()、componentWillUpdate()、componentDidUpdate() ：
>
> 会触发额外渲染，但必须等待本轮的更新生命周期执行完成（包括父组件），即执行完父组件的 componentDidUpdate() 函数

==状态何时改变（在 render() 函数之前）与调用哪些钩子函数压根就没关系==

==永远不要在组件内部使用 this.state = {}、this.props = {} 来修改状态和属性，行为是不可控的==

==状态使用 this.setState({})，属性永远都不要修改==

## **5.5 父子组件中对象传递**

​    本质上都是传递对象在栈中的引用值

​	结果：父基本类型  →（传值） → 子基本类型  /父对象  →（传引用） → 子对象

## **5.6 key 值**

​    父元素的销毁会带动其所有子元素的销毁（这种销毁是无理由的，强制的）

​    只要在同一级父组件下，两个子组件的 key 值就不能相同：兄弟组件之间的 key 值一定不能相同

​	key 值只能控制同类元素之间的更新

​		a. 同时有**同类/不同类**子组件 key 值相同：该行为是未定义的，不被支持，版本不稳定，行为将来会发生变化

​		b. 同类 -- 后渲染的子组件 key 值同先渲染的子组件 key 值相同：先渲染的**不会**销毁，将要渲染的将会在原来的基础上更新，只会执行 props 更新系列的生命周期函数

​		c. 不同类 -- 后渲染的子组件 key 值同先渲染的子组件 key 值相同：先渲染的**将会**销毁，但将要渲染的只会执行 **props 更新系列**的生命周期函数，并不会执行构造系列函数

## **5.7 子组件中同时修改父子组件状态**

​    父子组件状态同时发生更新，一定是先执行父组件更新

​    只会执行父组件状态更新生命周期函数即可，因为一定会触发子组件的属性更新生命周期函数

​    在子组件 componentDidMount() 中同时修改父子组件的状态，在执行完父组件 componentDidMount() 之后，同上，执行一次父组件的状态更新生命周期函数即可；

​    在子组件 componentWillReceiveProps(nextProps) 中同时修改父子状态，则

子组件状态本次更新时即可完成，父组件状态在下一轮完成

------

shouldComponentUpdate()、componentWillUpdate()、componentDidUpdate() 、componentDidUpdate():

均只会执行父组件的状态更新，主要看是否给子组件的状态更新留有机会接入

子组件的状态更新发生在

​						 componentWillReceiveProps(nextProps) 之后

​						shouldComponentUpdate()之前

## **5.8 Select 先设置 value = '123', 再设置 value = '123', label='aaa', 依旧可刷新显示 'aaa'**

## ==**5.9 state、props 初始化和更新完成事件**==

​	a. 挂载：在进入 construct(props) 之前已经完成， 其中 props 包括自己定义的属性和从父组件继承来的属性

​					在内部可以==**重置重置**== state,  props（重置 props 会有 warning, 一般不建议这么干）

​	b. 更新：在 componentWillUpdate()之后、render() 之前 完成

## ==**5.10 static getDerivedStateFromProps(props, state)**==

​	何时调用：初始化挂载、状态变化

​	a. static -- this: 由于是静态的，所以该函数内部无 this 属性，this 是 undefined

​	b. props 为当前所有的属性（包括内部的和外部传入的，还没真正更新到 this.props） -- newProps

​	b. state 为当前将要更新的所有状态（还没真正更新到 this.state）-- newState

​				ps. 组织一个完整的、将要更新的 newProps / newState

​	c. componentShouldUpdate() 中的 this.state, this.props 依旧没有更新完成，直到 render() 之前才完成更新

​	d. getDerivedStateFromProps 的返回值的作用主要是用于将 props 值来更新 state(更新同样直到 render() 才能完成更新)，利用 props 再次给 state 叠加一个属性值

调用：

初始化： constructor  →  （unsafe）willMount  →  getDerivedStateFromProps  →  render

属性更新/状态更新：getDerivedStateFromProps   →  shouldUpdate  →  willUpdate  →  render

## ==**5.10 状态的重置与修改**==

​	a. 重置：只会在一个地方重置，就是在 constructor(props) { this.state = {} } 会重置状态

​	b. 更新： 其余像 setState()、static getDerivedStateFromProps(props, state) {return {}} 均是更新

> ​	ps. 更新包括 修改 和 添加
>
> ​			状态 state 只能更新，不能重置 或者 删除某个属性
>
> ​	setState({}, 回调函数)：其中回调函数在 componentDidMount() 之后才会执行

## **5.11 组件**

UI组件 -- components   容器组件 -- containers  路由组件 -- pages/views

静态组件/动态组件、受控组件/非受控组件

# 6. antd 中 Form 的用法

```react
const { form } = this.props;
const { getFieldDecorator } = form;
<Form>
    <Form.Item >
        { getFieldDecorator('MyInput', {      // option
            initialValue: 'testValue',
            trigger: 'onChange',
            // 作用同 valuePropName， 但作用强于它
            // getValueProps: (num10) => ({ num10 }),   
            valuePropName: 'value',                  	// 子组件受控的属性
            getValueFromEvent: function f(e) {
              if (!e || !e.target) {
                return e;
              }
              const { target } = e;
              return target.type === 'checkbox' ? target.checked : target.value;
            },
            rules: [      // 校验规则
                {
                    required: true,
                    message: '请设置非空值',
                },
            ],
        }
        )(
            <MyInput form={form} num1={10} num2={20} />
        )
        }
    </Form.Item>
</Form>
```

> id: 随意设置
>
> option:
>
> ​	initialValue: 包裹元素的初始值，不是元素第一次挂载的时候使用，而是以当前 id 第一次创建 						  Form 的时候使用，其他时候有值，自然直接用该 id 对应的值
>
> ​						  只会在第一次挂载的时候使用，后续即使将 id 的值设置为 undefined，传递给子组						  件的 value 值依旧会是 id 值：undefined
>
> ​	trigger： 默认值 String = 'onChange'，子组件触发回调函数的实际
>
> ​	getValueProps: (x) => ({ x}): 作用同 valuePropName, 但是作用优先级高于 valuePropName
>
> ​	valuePropName: 默认值 String = 'value'
>
> ​	当 trigger 指定的动作被触发时，执行的动作函数： 以下为默认值
>
> ```react
> getValueFromEvent: function f(e) {
>  if (!e || !e.target) {
>      return e;
>  }
>  const { target } = e;
>  return target.type === 'checkbox' ? target.checked : target.value;
> },
> ```
> 更新时值为什么会保持不变？
>
> ​	因为值被维护在 getFieldDecorator 中的 id 字段属性中，每次更新时都传递该属性

子组件触发（一般是值发生变化）  →  trigger: 决定触发实际    →   getValueFromEvent: 触发时执行的函数，将(onChange) 的参数(如 event) 转化为控件的值    →    改变 form 状态   →  将 x 作为属性值传给被包裹组件

**==自定义第三方组件要求：==**

> 1. 提供受控属性 value 或其他与 valuePropName 同名属性
> 2. 提供 onChange 事件或 trigger 值同名事件

==**通过 getFieldDecorator 包装组件，表单组件自动添加两个属性：**==

> 1. value 或者 valuePropName 指定的属性
> 2. onChange 或 trigger 指定的其他同名事件属性， 其值 = getValueFromEvent

value 值为 undefined 时，Select 组件会降级显示到 placeholder 的值

# 7. Promise 对象

> axios 发送的异步请求返回的是一个 promise 对象

## 7.1. 返回 Promise 对象的原因

> javascript 是单线程语言，所以都是同步执行的，
> 要实现异步就得通过回调函数，但是过多的回调会导致回调地狱，代码既不美观，也不易维护，
> 所以就有了 Promise;

## 7.2. Promise 用法

- Promise 是一个代理对象，代理一个值，这个值可以称为 Promise 对象的状态

- 三种状态：pending、resolved、rejected

- 创建一个 Promise 对象语法：

  ```react
  new Promise(function(resolve, reject) {...})
  .then(function(res) {})
  .catach(function(err) {})
  .finally(function() {});
  ```

  1. 构造函数传入的 function 函数：称为 executor 执行器，reject。

  2. 当创建一个新的 Promise对象时，==executor 会立即同步执行执行==。在 executor 方法的方法体内，可以写自己的代码逻辑，一般逻辑代码都包括正常执行逻辑和出错异常处理，

     ==a. 在代码的正常执行逻辑里调用  resolve(retValue)  把 Promise 的 status 改为 resolved==

     ==b. 在出错异常处理的代码里调用 reject(err) 把 Promise 的 status 改为 rejected==

## 7.3 Promie 源码（自己写的）

```react
class Promise {

    // then 函数
    // 用法：一定会返回一个新的 promise 对象
    // 状态：由调用者 promise 自身状态和 回调函数的返回值共同决定
    // promise 状态：
    // 'pending': 保持不变，不调用 resolve() 和 reject() 函数
    // 'rejected': 不会进入 回调函数，直接 reject 原来 promise 状态值
    // 'resolved': 进入回调函数，根据回调函数返回值决定新的 promise 状态 
    then(func) {

        return new Promise((resolve, reject) => {
            setTimeout(() => {

				
                if (this.PromiseStatus === 'rejected') {
                    reject(this.PromiseStatus)
                } else if (this.PromiseStatus === 'resolved') {
                    try {
                        let tmp = func(this.PromiseValue);
                        if (tmp instanceof Promise) {
                            if (tmp.PromiseStatus === 'pending') return;
                            else if (tmp.PromiseStatus === 'resolved') 													resolve(tmp.PromiseStatus)
                            else if (tmp.PromiseStatus === 'rejected') 													reject(tmp.PromiseStatus)
                        } else {
                            resolve(tmp);
                        }
                    } catch (error) {
                        reject(error)
                    }
                  
                    
                    
                    
                }
            }, 0);
        });
    }


    // catch 函数
    // 用法：一定会返回一个新的 promise 对象
    // 状态：由调用者 promise 自身状态和 回调函数的返回值共同决定
    // promise 状态：
    // 'pending': 保持不变，不调用 resolve() 和 reject() 函数
    // 'then': 不会进入 回调函数，直接 resolve 原来 promise 状态值
    // 'rejected': 进入回调函数，根据回调函数返回值决定新的 promise 状态 
    catch(func) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                if (this.PromiseStatus === 'pending') {
                    return;
                } else if (this.PromiseStatus === 'resolved') {
                    resolve(this.PromiseStatus)
                } else if (this.PromiseStatus === 'rejected') {
                    try {
                        let tmp = func(this.PromiseValue);
                        if (tmp instanceof Promise) {
                            if (tmp.PromiseStatus === 'pending') return;
                            else if (tmp.PromiseStatus === 'resolved') 												resolve(tmp.PromiseStatus)
                            else if (tmp.PromiseStatus === 'rejected') 												reject(tmp.PromiseStatus)
                        } else {
                            resolve(tmp);
                        }
                    } catch (error) {
                        reject(error)
                        throw error;
                    }
                }
            }, 0);
        });
    }
}

```

**用法总结：**

```react
// 后续的每一个 catch() 函数调用都会返回一个同初始 promise 同状态和状态值的 promise
// 相当于 初始 promise 一直在向后传递，直到遇到第一个 .then() 才会去执行它的回调函数
    new Promise((resolve, reject) => { resolve('zx') })
    .catch(f1).catch(f2).catch(f3)...
```

```react
// 后续的每一个 then() 函数调用都会返回一个同初始 promise 同状态和状态值的 promise
// 相当于 初始 promise 一直在向后传递，直到遇到第一个 .catch() 才会去执行它的回调函数
new Promise((resolve, reject) => { reject('zx') })
.then(f1).then(f2).then(f3)...
```



```react
// 将两个 Promise 状态关联起来，testB2 的状态会随着 testB1 变化而变化
testB = new Promise((resolve, reject) => {

});
testB2 = new Promise(function (resolve, reject) {
	resolve(testB);
})
```































## 7.2. Promise 对象状态

Promise 对象代表一个异步操作，有三种状态：

pending（进行中）、resolved(fulfilled)（已成功） 和 rejected（已失败）

> 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态
>
> Promise 对象状态改变，只有两种可能：pending --> fulfilled 、pending --> rejected

##### a. 用法

==function(resolve, reject) {...} 表示 Promise 请求返回之后执行的回调函数（不管成功还是失败，都执行这个函数，默认会根据请求成功还是失败调用 resolve 或者 reject）==

```react

```

> ​    一个 promise 可以注册多个 .then  .catch 函数
> ​    同一批注册的 .then/.catch 函数，注意下面这句话是针对同一批注册的：
> ​    请求成功/请求失败的结果只会传递给同一批注册中第一个注册的 .then/.catch 函数，其余获取不到返回结果
> ​    Promise 对象的状态改变之后，你再给它添加回调函数，这个函数也会执行。
> ​    跟事件监听器很不一样 — 你一旦错过某个事件，就没办法再捕获他了，除非这个事件再次发生。

##### b. 请求成功/请求失败返回的数据格式（必须手动取出 data）

> 请求成功：

```json
response: {
	config: {url: "/ssm_war_exploded/account/selectAccountByUsername", method: 				"get", params: {…}, headers: {…}, transformRequest: Array(1), …}
	data: {id: 1, username: "zx", password: "aaa"}  // 响应得到的数据
	headers: {date: "Tue, 29 Oct 2019 01:47:06 GMT", content-encoding: "gzip", 					vary: "Accept-Encoding", connection: "close", x-powered-by: 					"Express", …}
	request: XMLHttpRequest {onreadystatechange: ƒ, readyState: 4, timeout: 0, 		withCredentials: false, upload: XMLHttpRequestUpload, …}
	status: 200
	statusText: "OK"
}
```

> 请求失败：

```json
error: {
    config: {url: "/ssm_war_exploded/account/selectAccountByUsername1", method: 			"get", params: {…}, headers: {…}, transformRequest: Array(1), …}
    isAxiosError: true
    request: XMLHttpRequest {onreadystatechange: ƒ, readyState: 4, timeout: 0, 					withCredentials: false, upload: XMLHttpRequestUpload, …}
	response: {data: "<!doctype html><html lang="zh"><head><title>HTTP S…ne" />					<h3>Apache Tomcat/9.0.22</h3></body></html>", status: 404, 						statusText: "Not Found", headers: {…}, config: {…}, …}
    toJSON: ƒ ()
	message: "Request failed with status code 404"    // 输出的错误信息
	stack: "Error: Request failed with status code 404↵    at createError 					(http://localhost:3000/static/js/0.chunk.js:45419:15) at settle 				(http://localhost:3000/static/js/0.chunk.js:45635:12) at 						XMLHttpRequest.handleLoad 							 	
			(http://localhost:3000/static/js/0.chunk.js:44942:7)"
	__proto__: Object	
} 
```

==Promise的设计文档中说了，[[PromiseValue]]是个内部变量，外部无法得到，只能在then中获取==

then中传了两个参数，then方法可以接受两个参数，第一个对应 resolve 的回调，第二个对应 reject 的回调。所以我们能够分别拿到他们传过来的数据。

**说明：Chrome 中打印promise对象的时候，`[[PromiseStatus]]`有三个值：pending，resolved，rejected，这里的`resolved`状态就是指 `fulfilled` 状态，和我们要说明的`resolved` 没有关系，在 firefox 中，`[[PromiseStatus]]`三个值和标准相同：pending，fulfilled，rejected。为了避免混淆，用firefox做测试**



> await用法：
>
> 1. await a(一个 promise 对象)：等待 promise 对象的状态变为 fulfilled，返回 promise 对象的值
> 2. 其余任何状态都不会返回，await 会一直阻塞 async 函数，直到 promise 对象的状态变为 fulfilled





# 7. ajax 与 axios 用法：

## 7.1. JavaScript 发送原生 ajax 请求

### 7.1.1. get 请求

```react
// 步骤1：创建异步对象
var ajax = new XMLHttpRequest();
// 步骤2：设置请求参数
// 参数1: 请求类型，
// 参数2: 请求 url，可以带参数，动态的传递参数 name=zx&age=13 到服务端
// 参数3：决定 send() 同步执行还是异步执行
ajax.open("get", "ajaxTest?name=zx&age=13", true);
//步骤3：发送请求，get 请求不需要发送请求体
ajax.send();

// 步骤4：注册事件 onreadystatechange  状态改变就会调用
ajax.onreadystatechange = function() {
    if(ajax.readyStatus == 4 && ajax.status == 200){
        //步骤5：如果能够进入到这个判断，说明数据完美的返回回来了，并且请求的页面是存在的
        console.log(ajax.responseText);
    }
}
```

### 7.1.2. post 请求

```react
// 步骤1：创建异步对象
var xhr = new XMLHttpRequest();
// 步骤2：设置请求参数
// post 请求一定要添加请求头才行，不然会报错，参数3：决定send()同步执行还是异步执行
// 2.1 发送 Form Data 表单格式数据
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xhr.open("post", "ajaxTest?name=zx&age=13");
xhr.send("a=1&b=1ab3");
// 2.2 发送 Json 格式数据
xhr.setRequestHeader("Content-type","application/json; charset=utf-8");
xhr.open("post", "ajaxTest?name=zx", true);
xhr.send(JSON.stringify({a: 1, b: "1ab3"}));      // 这里必须要把对象 序列化 成字符串

// 步骤3：注册事件 onreadystatechange  状态改变就会调用
xhr.onreadystatechange = function() {
    //这步为判断服务器是否正确响应
    if(xhr.readyState == 4 && xhr.status == 200){
        //根据服务器的响应内容格式处理响应结果
        if(xhr.getResponseHeader('content-type')==='application/json') {
            // Json 格式：'{"a": 1, "b": "bValue"}' 
            var result = JSON.parse(xhr.responseText);	  
        } else {
        	console.log(xhr.responseText);
        }
    }
};
```

   ====ajax 技术实现了网页的局部数据刷新，axios 实现了对 ajax 的封装==
   ==axios 封装了 ajax 的部分功能，ajax 远不止 axios==

## 7.2. axios 封装 js 中原生的 ajax 请求

### 7.2.1. axios 发送 get 请求

```react
// 1. 直接在 url 中带参数
axios.get('/testGet1?a=1&b=bbb')
    .then(response => {
    console.log(response);
}).catch(err => {
    console.log(err);
})

// 2. 将 params 中的参数解析出来 追加到 url 请求后面)， url = '/testGet2?x=10&a=1&b=bbb'
axios.get('/testGet2?x=10', { params: { a: 1, b: 'bbb' } })
    .then(response => {
    console.log(response);
}).catch(err => {
    console.log(err);
})

// 3. 同方式1：不会去解析 data 数据，get 请求所有参数都在 url 中
const getOptions = {
    url: '/testGet2?x=10',
    method: 'GET',
    // data: {params: {a: 1, b: 'bbb'}},    
};
axios(getOptions);
```

### 7.2.2. axios 发送 post 请求

```react
// 1. post 请求默认是以 Content-Type: application/json;charset=UTF-8 格式发送
axios.post('/testPost', { a: 1, b: 'bbb' })
.then(response => {
    console.log(response);
}).catch(err => {
    console.log(err);
})

// 2. 
// 2.1 未设置属性 headers 时
// data 为对象：Content-Type: application/json;charset=UTF-8
// data 为字符串：Content-type: application/x-www-form-urlencoded
const postOptions = {
    url: '/testPost1?x=10',
    method: 'POST',
    data: {},  // data: '',
    };
axios(postOptions);

// 2.2 设置属性 headers 时
// Content-Type: application/json;charset=UTF-8 ： data 必须是对象 {a: 1, b: 'bbb'}
// Content-Type: application/x-www-form-urlencoded ： data 必须是字符串 'a=1&b=bbb'
const postOptions2 = {
    url: '/testPost2?x=10',
    method: 'POST',
    headers: { 'content-type': 'application/x-www-form-urlencoded' },
    // data: 能够自动将 data 对象序列化 
    data: 'a=1&b=bbb',    // data: {a: 1, b: 'bbb'}          
};
axios(postOptions2);
```

### 7.3. Promise 对象

> axios 发送的异步请求返回的是一个 promise 对象

#### 7.3.1. 返回 Promise 对象的原因

> javascript 是单线程语言，所以都是同步执行的，
>  要实现异步就得通过回调函数，但是过多的回调会导致回调地狱，代码既不美观，也不易维护，
> 所以就有了 Promise;

#### 7.3.2. Promise 对象状态

Promise 对象代表一个异步操作，有三种状态：

pending（进行中）、resolved(fulfilled)（已成功） 和 rejected（已失败）

> 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态
>
> Promise 对象状态改变，只有两种可能：pending --> fulfilled 、pending --> rejected

##### a. 用法

==function(resolve, reject) {...} 表示 Promise 请求返回之后执行的回调函数（不管成功还是失败，都执行这个函数，默认会根据请求成功还是失败调用 resolve 或者 reject）==

```react
// resolve 表示 then 中的回调函数：function(res) {}, 一旦异步请求成功，则执行resolve  
// reject表示catch中的回调函数：function(err) {}, 一旦异步请求失败，则执行reject  默认
new Promise(function(resolve, reject) {...})
.then(function(res) {})
.catach(function(err) {})
.finally(function() {});
```

> ​    一个 promise 可以注册多个 .then  .catch 函数
> ​    同一批注册的 .then/.catch 函数，注意下面这句话是针对同一批注册的：
> ​    请求成功/请求失败的结果只会传递给同一批注册中第一个注册的 .then/.catch 函数，其余获取不到返回结果
> ​    Promise 对象的状态改变之后，你再给它添加回调函数，这个函数也会执行。
> ​    跟事件监听器很不一样 — 你一旦错过某个事件，就没办法再捕获他了，除非这个事件再次发生。

##### b. 请求成功/请求失败返回的数据格式（必须手动取出 data）

> 请求成功：

```json
response: {
	config: {url: "/ssm_war_exploded/account/selectAccountByUsername", method: 				"get", params: {…}, headers: {…}, transformRequest: Array(1), …}
	data: {id: 1, username: "zx", password: "aaa"}  // 响应得到的数据
	headers: {date: "Tue, 29 Oct 2019 01:47:06 GMT", content-encoding: "gzip", 					vary: "Accept-Encoding", connection: "close", x-powered-by: 					"Express", …}
	request: XMLHttpRequest {onreadystatechange: ƒ, readyState: 4, timeout: 0, 		withCredentials: false, upload: XMLHttpRequestUpload, …}
	status: 200
	statusText: "OK"
}
```

> 请求失败：

```json
error: {
    config: {url: "/ssm_war_exploded/account/selectAccountByUsername1", method: 			"get", params: {…}, headers: {…}, transformRequest: Array(1), …}
    isAxiosError: true
    request: XMLHttpRequest {onreadystatechange: ƒ, readyState: 4, timeout: 0, 					withCredentials: false, upload: XMLHttpRequestUpload, …}
	response: {data: "<!doctype html><html lang="zh"><head><title>HTTP S…ne" />					<h3>Apache Tomcat/9.0.22</h3></body></html>", status: 404, 						statusText: "Not Found", headers: {…}, config: {…}, …}
    toJSON: ƒ ()
	message: "Request failed with status code 404"    // 输出的错误信息
	stack: "Error: Request failed with status code 404↵    at createError 					(http://localhost:3000/static/js/0.chunk.js:45419:15) at settle 				(http://localhost:3000/static/js/0.chunk.js:45635:12) at 						XMLHttpRequest.handleLoad 							 	
			(http://localhost:3000/static/js/0.chunk.js:44942:7)"
	__proto__: Object	
} 
```

### 7.4. axios 具体的封装做法

##### a. 最原始写法

> ==这里的返回值 response 包含着返回值的所有内容==
>
> ==需要使用 response.data 获取返回的内容==

```react
// 最底层封装：返回 Promise 对象
export default function ajax(url, data = {}, type = 'GET') {
    if (type === 'GET') { 		  // 发送GET请求
        return axios.get(url, {   // 配置对象
        params: data,         	  // 指定请求参数
    });
    } else if (type === 'POST') {
    	return axios.post(url, data);
    }
}
// 应用层封装：返回promise对象
export function reqLogin(username, password) {
    return ajax('/ssm_war_exploded/account/selectAccountByUsername', 
                { username,password }, 
                'GET');
}
// 应用：为返回的promise对象注册请求成功失败回调函数
reqLogin(username, password).then(response => {
	console.log(response);	
}).catch(err => {
	console.log(err);
}) 
```

##### b. 利用async、await，消灭回调函数，以同步的方式写异步代码

**==async 标记的函数表明函数在此次 异步请求中异步的是整个函数==**

==也就是说当 async 标记的函数执行到 await 标记的地方时，会阻塞整个函数的执行，从而跳出当前函数执行函数外面的内容，知道 await 标记的函数返回时，再次回调 async 标记的被阻塞的函数==

==这里的返回值 response 依旧包含着返回值的所有内容==

==需要使用 response.data 获取返回的内容==

> 1. 作用？
>    简化promise对象的使用：不用再使用.then()来指定成功/失败的回调函数
>     以同步编码方式实现异步流程（简单的说，就是没有使用回调函数，一旦使用回调函数，就是异步编码方式）
> 2. 哪里写 await?
>    promise 的表达式左侧写 await：不想要 promise，想要 promise 异步执行成功的 data 数据
> 3. 哪里用 async
>    await 所在函数（最近的）定义的左侧写 async

```react
// 最底层封装：返回promise对象
export default function ajax(url, data = {}, type = 'GET') {
    if (type === 'GET') { // 发送GET请求
        return axios.get(url, {   // 配置对象
            params: data,         // 指定请求参数
        });
    } else if (type === 'POST') {
        return axios.post(url, data);
    }
}
// 应用层封装：返回promise对象
export function reqLogin(username, password) {
    return ajax('/ssm_war_exploded/account/selectAccountByUsername', 
                { username, password }, 
                'GET');
}
// 应用：为返回的promise对象注册请求成功失败回调函数
async f() {
    try {
        // 相当于：const response = reqLogin(username, password)
        //                           .then(response => { return response })
        //                           .catch(error => { console.log('error', 			//                             				   	error.message); })
        const response = await reqLogin(username, password);
        console.log(response);
    } catch (error) {
        console.log('error', error.message);
    }
    const result = response.data;      // 获取返回值中的 data 数据
}
```

##### c. 进一步优化，统一处理请求异常

> 优化：同一处理请求异常
>  在外层包一个自己创建的promise对象
>  在请求出错时，不去reject(error)，而是显示错误提示

```react
// 最底层封装：返回包裹内部异步请求返回的promise的promise对象
export default function ajax(url, data = {}, type = 'GET') { 
    return new Promise((resolve, reject) => {
    	let promise;
    	// 1. 执行异步ajax请求
    	if (type === 'GET') { // 发送GET请求
            return axios.get(url, {   // 配置对象
                params: data,         // 指定请求参数
            });
    	} else if (type === 'POST') {
    		promise = axios.post(url, data);
    	}
        // 2. 如果成功了，调用resolve(value)
        promise.then(response => {
            // 此处的resolve是外层包裹的promise的resolve
            // 相当于resolve = .then(response => { return response })
            resolve(response.data);
        }).catch(error => {        
            // 3. 如果失败了，不调用reject(error)--并没有注册reject，而是提示异常信息
            message.error('请求出错了: ' + error.message);
        })
   });
}
// 应用层封装：返回promise对象
export function reqLogin(username, password) {
    return ajax('/ssm_war_exploded/account/selectAccountByUsername', 
                { username, password }, 
                'GET');
}
// 应用：为返回的promise对象注册请求成功失败回调函数
async f() {
    // 相当于：const response = reqLogin(username, password)
    //                           .then(response => { return response })
    // 未注册错误处理函数：reject(error)
    const result = await reqLogin(username, password);
}
```

## 7.5. 高阶函数与高阶组件

### 7.5.1. 高阶函数

> 定义：a. 接收函数类型的参数  b. 返回函数类型的返回值

> 常见的高阶函数：
>            接收函数类型的参数：
>            a. 定时器：setTimeOut()：只执行一次/setInterval()：循环执行
>            b. Promise: Promise(() => {}).then(value => {}, reason => {})
>            c. 数组遍历相关的方法：forEach()  map()  filter()  reduce()  find()  findIndex()
>            返回函数类型的参数：
>            d. 函数对象的bind方法：this.fn.bind(this) // 返回与对象绑定的函数
>            接收函数类型参数，返回函数类型参数（接收组件函数，返回组件函数）
>            e. Form.create()

> 高阶函数更加动态、更加具有扩展性

### 7.5.2. 高阶组件

> 1). 本质上就是一个函数，同时也是一个高阶函数，组件本质上就是一个函数
> 2). 接收一个组件（被包装的组件），返回一个新的组件（包装组件），
>        a. 父子关系：父组件负责向子组件传递属性
>        b. 新组件内部渲染被包装的组件，包装组件会向被包装组件传入特定属性
>        c. 传递的是组件，而不是组件标签
>        d. 组件--类型：创造一个组件标签的对象（函数）   组件标签--实例：实例化的一个对象
>        e. Form.create()的返回值是一个高阶组件
> 3). 扩展组件的功能
> 4). 高阶组件也是一个高阶函数，接收一个组件函数，返回一个组件函数
> 5). 包装Form组件，生成一个新的组件：<Form(Login)></Form(Login)>
> 6). 新组建会向Form组件传递一个强大的对象属性：form

- 进程是cpu资源分配的最小单位（是能拥有资源和独立运行的最小单位）

- 线程是cpu调度的最小单位（线程是建立在进程的基础上的一次程序运行单位，一个进程中可以有多个线程）

- reject() 抛出的异常是一个宏任务

- await 会在后面的表达式执行完成之后：

  - 非promise对象：表达式执行完成
  - promise对象：promise.then().catch等执行完

  将 await 语句后面的语句加到 微任务队列 中

- reject 抛出的异常属于宏任务还是微任务，暂时还不确定，各个浏览器的实现好像并不兼容，有所不同，但是至少可以肯定的是，至少是在微任务全部执行完之后才会去处理

# 8. 函数 this 绑定

## 8.1. ~~全局作用域函数默认绑定~~

```js
function foo(){
    console.log(this.a);
}
var a = 2 ;
foo(); 			// 2 this 绑定到 window 对象，this.a = window.a = 2
```

> 调用 `foo()` 的时候其实相当于 `window.foo()`，所以 `this.a` 其实指向的是 `window.a`

```javascript
window = {
	foo: function (){
    	console.log(this.a);
	},
	a: 2,
}
```

对象中的函数 `this` 是绑定到当前对象上

```javascript
const o = {
	a: 10,
	foo: function () {
		console.log(this.a);
	}
}
o.foo();     // 10
```

==`this` 到底绑定到什么地方，是根据代码执行时的上下文对象决定的（不是根据函数定义时的上下文对象决定的）==

```javascript
function foo(){
    console.log( this.a );
}
const o = {
	a: 10,
	foo: foo,
}
o.foo();
```

## 8.2. 显式绑定

利用函数的 `apply` 、`call` 、`bind` 显示的将对象绑定到调用函数内部的 `this`

```javascript
const a = {
    name: 'zx',
    age: 13,
    weight: 74.9,
}

function foo() {

    const keys = Object.keys(this);
    keys.forEach((key) => {
        console.log(key, this[key])
    })
}
// apply
foo.apply(a);
// call
foo.call(a);
// bind: 返回一个新的函数
const fooBind = foo.bind(a);
fooBind();
```

## 8.3. new

1. 调用  `[[construct]]`  创建一个新的对象；
2. 这个新对象会绑定到函数调用的 this，即将该构造函数的作用域赋给新对象；
3. 调用 `[[call]]` 执行函数体中的代码
4. 返回新对象，即在函数体中执行的 `this` 对象

```javascript
function foo() {
	this.name = 'zx';
	this.age = 13;
}
console.log(new foo());
```





> `const`,  `let` 定义的变量没有添加属性到 `this` 中，但其作用域与生存期的概念同 `c/c++` 完全相同

在函数中使用 `this.a` 与 `a` 是不一样的意思：

> - `this.a` : `this` 指的是调用该函数的对象，`this.a` 表示在调用该函数的对象中查找 `a`
> - `a` ：此处的 `a` 的查找是在函数执行的作用域链中查找，与 `this` 毫无关系  

```javascript
var a = 10;

function foo () {
	var a = 999;    // 标记：1
    var o = {
        a: 1000,
        foo: function () {
            console.log(this.a);
            console.log(this.b);
            console.log(a);       // 如果注释掉标记1处代码，则输出 10
        }
    };

    return o;
}

foo().foo();    // 1000   undefined   999
```

> ==核心原理：在函数作用域中，一定会有一个 `this` 变量，该变量与调用该函数的对象有关==

```
// f 里面的 this 就是指 a，不管 f 是个什么玩意
a.f();
// (不考虑bind，以及严格模式时情况下)，f 里面的 this 就是指全局对象 window。
f();
```

==在任何语言中，变量的存储都是理解语言的基础，变量作为一门语言最为关键的组成部分，有必要深究，更有必要理解，JavaScript 的基础类型真的是存在栈中的吗？==





Whenever a function is called, we must look at the immediate left side of the brackets / parentheses “()”. If on the left side of the parentheses we can see a reference, then the value of “this” passed to the function call is exactly of which that object belongs to, otherwise it is the global object. 







```javascript
class A {
    constructor() {
        this.a = 10;
    }

    getA() {}          // 相当于 A.prototype.getA = function getA() {} 

    getB = () => {}    // 相当于在函数体中：this.getB = () => {}
}

var x = new A();
var y = new A();


console.log(x.getA === y.getA);
console.log(x.getB === y.getB);
```

等价于

```
a.prototype.getA = function () {}
function a() {
    this.getB = () => {
        console.log(this);
    }
    this.getC = function () {
        console.log(this);
    }
}

var x = new a();

var m = x.getB;
var n = x.getC;
m();     // Object { getB: getB(), getC: getC() }
n();     // Window 
```

> 如果箭头函数被包含在一个非箭头函数内，那么 this 值就会与该函数的相等；否则，
> this 值就会是全局对象  
>
> 箭头函数同样会有闭包





```javascript
// let 变量会发生直接拷贝，但拷贝的只是局部栈里面的内容
var funcs;
let x;
for (let i = {a: 10, b: 30}; i.a < 11; i.a++) {
    x = i;
    func = function () { 
        console.log(i); 
        return i;
    } ;
}
console.log(x === func()); 
// 输出
// Object { a: 11, b: 30 }
// true
```

**很奇怪**

```javascript
var x = [];
function foo() {
    if (true) {
        let i = 0;
        while (i < 3) {
            x[i] = function () {
                console.log(i);
            }

            i = i + 1;
        }
    }

}

foo();
x[0]();  // 3
x[1]();  // 3

// 只有使用 for(int i = 0; i < 3; ++i) 最终才会输出 0 1 2
// 怀疑：当存在闭包的时候，会导致 词法作用域（块作用域）不会释放，for的用法很特殊
```

