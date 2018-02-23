

   react 相关知识
   
   state 的主要作用是用于组件保存、控制、修改自己的可变状态。state 在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为 state 是一个局部的、只能被组件自身控制的数据源。state 中状态可以通过 this.setState 方法进行更新，setState 会导致组件的重新渲染。
   props 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的 props，否则组件的 props 永远保持不变。
   state 和 props 有着千丝万缕的关系。它们都可以决定组件的行为和显示形态。一个组件的 state 中的数据可以通过 props 传给子组件，一个组件可以使用外部传入的 props 来初始化自己的 state。但是它们的职责其实非常明晰分明：state 是让组件控制自己的状态，props 是让外部对组件自己进行配置。
   如果你觉得还是搞不清 state 和 props 的使用场景，那么请记住一个简单的规则：尽量少地用 state，尽量多地用 props。
   没有 state 的组件叫无状态组件（stateless component），设置了 state 的叫做有状态组件（stateful component）。因为状态会带来管理的复杂性，我们尽量多地写无状态组件，尽量少地写有状态的组件。这样会降低代码维护的难度，也会在一定程度上增强组件的可复用性。前端应用状态管理是一个复杂的问题，我们后续会继续讨论。
   
   当调用setState的时候，Reactjs并不会马上修改state，会将对象放到一个更新队列中，然后从队列中提取合并到state并触发组件更新。setState接受一个函数作为参数，Reactjs会把上一个setState的结果传入这个函数，可以对该结果做运算、操作，更新state。
   
   
   理解：组件之间共享一些数据，需要将共享的数据（状态）交给组件最近的公共父节点保管，通过props传递给子组件，实现数据的共享。但这不是一个好的解决方案。状态管理需要借助redux。
   
   ->constructor() 初始化
   ->componentWillMount() 进行组件的启动工作，例如ajax数据拉取，定时器启动
   ->render()
	->componentDidMount() 需要依赖DOM时在这里处理，因为Will还未挂载
	->componentWillUnmount() 数据清理

	组件可以认为是继承自父类React.Component的一个子类，除了拥有父类的一些方法（如各种生命周期函数）外，还可以自定义一些方法供内部使用。
	组件的生命周期函数可以重写也可以不重写。如果重写，则会覆盖从父类继承过来的生命周期函数；如果不重写，则默认就使用从父类继承过来的生命周期函数。
	render方法也是继承自React.Component，每个组件一定要有，用于输出组件。
	render方法返回的是一个虚拟dom节点，是一个javascript对象
	render方法在组件创建期（仅一次）、更新期（可重复）均会被调用
	render方法只能返回一个顶级组件，不能返回一组元素
	在React的应用中，还会有ReactDOM.render()方法，这个方法一般在页面最顶层组件才会使用，它会将虚拟DOM渲染为真实DOM。而普通组件（extends React.Component）的render()方法返回的都是虚拟DOM。
	
	
	
	组件的属性和状态是基本的概念，但是两者有诸多差别，理解他们的定义和差别非常重要
	属性（props）
	属性对于组件之间的通信非常重要。可以通过属性从父组件传递数据到子组件。
	当父组件传递的属性数据改变时，React会向下遍历整个组件树，重新渲染使用了这个props的组件。
	在组件内部，this.props是只读的，不能在组件内部修改this.props。
	状态（state）
	state用来表示组件的内部状态，随着与用户的互动可对state进行修改，但是只能在组件内部修改。
	state往往在constructor中进行初始化，在组件中其它地方可以通过this.setState()方法进行修改。只能在组件内部使用，属于组件私有。
    setState()方法不会直接改变组件的状态，而是创建了一个异步的状态转移。如果在调用setState()方法后访问this.state，那么可能得到的只是原来的状态，而不是改变后的状态。不能保证对setState()调用的同步操作。setState()将总是触发重新渲染，除非在shouldComponentUpdate()中返回了false。
	属性和状态的比较
	props和state都是纯javascript对象，对它们的改变都会触发render更新
	组件的render方法依赖于this.props和this.state，React会确保渲染出来的UI界面总是与输入（this.props和this.state）保持一致。
	props不能在组件内部修改，可以在组件外部通过setProps()修改，但是几乎不用。
	state只能在组件内部修改，不能在组件外部直接对其修改。


	组件的生命周期主要包括3个阶段：Mount（创建期），Update（更新期），Unmount（销毁期）。
	React为每个生命周期阶段都提供了两种处理函数，will函数在进入状态之前调用，did函数在进入状态之后调用。
	React还提供了两种特殊状态的处理函数
	componentWillReceiveProps(nextProps): 当已加载组件收到新的props时调用
	shouldComponentUpdate(nextProps, nextState): 组件判断是否重新渲染时调用。默认返回true，一般情况下，就算重写也返回true。如果设为false，那么在更新期组件将不会执行render重新渲染。


    React中组件的通信方式完全类似
	父组件 -> 子组件：通过设置属性值，父组件把数据通过props传递给子组件。
	子组件 -> 父组件：使用回调函数作为子组件的属性实现子组件向父组件的数据传递。
	无父子关系的组件：事件系统，订阅发布模式。或者还是子组件先更新数据到父组件，然后组件再更新到子组件。	
	
	
	（1）每次调用时，显示地为函数绑定上this对象。
	import React from 'react';
	class App extends React.Component {
		constructor(props) {
		super(props);
		}
		handleClick() {
			console.log(this); // React Component instance
			this.setState({}); // will work correctly
		}
		render() {
			return (
				<div onClick={this.handleClick.bind(this)}></div>
			);
		}
	}
	export default App;
    （2）在constructor中为了函数统一绑定上this对象，这样以后每次调用时无需再重复绑定，这种方式更好一点，一次绑定后，后续无需再关注。
	import React from 'react';
	class Contacts extends React.Component {
		constructor(props) {
		super(props);
		this.handleClick = this.handleClick.bind(this);
		}
		handleClick() {
			console.log(this); // React Component instance
			this.setState({}); // will work correctly
		}
		render() {
			return (
				<div onClick={this.handleClick}></div>
			);
		}
	}
	export default Contacts;

	
	在React中，当组件状态 state 有更改的时候，React 会自动调用组件的 render 方法重新渲染整个组件的 UI。
	当然如果真的这样大面积的操作 DOM，性能会是一个很大的问题，所以 React 实现了一个Virtual DOM，组件 DOM 结构就是映射到这个 Virtual DOM 上，React 在这个 Virtual DOM 上实现了一个 diff 算法，当要重新渲染组件的时候，会通过 diff 寻找到要变更的 DOM 节点，再把这个修改更新到浏览器实际的 DOM 节点上，所以实际上不是真的渲染整个 DOM 树。这个 Virtual DOM 是一个纯粹的 JS 数据结构，所以性能会比原生 DOM 快很多。
			
	
	Webpack 是当下最热门的前端资源模块化管理和打包工具，只需要相对简单的配置就可以提供前端工程化需要的各种功能，并且如果有需要它还可以被整合到其他比如 Grunt / Gulp 的工作流。
	Webpack可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。
	Webpack加载器的最终目的，是将各种资源，打包成静态代码（js/css/html）以及静态资源（图片、字体文件、文件等）。
	
	
	npm是node的模块管理器，安装nodejs后，会自动安装对应的npm版本。有了npm，我们只需自行一条命令，即可安装别人写好的模块，模块相关的依赖也自动被安装上。npm是前端模块化开发的maven，功能与maven类似，在配置文件中package.json(等同maven的pom.xml)中指定相关模块的版本信息，执行npm install即可自动安装相关模块。
	npm同样可以构建自己的源，在企业内部连接自己搭建的源服务，可以解决无外网权限的问题，同时也可以配置proxy代理来访问外网公共库。
	
	
	
	
		
		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
