---
date: "2025-08-18T22:31:12+08:00"
draft: false
title: "js-dom"
tags: ["深入浅出", "虚拟 DOM"]
categories: ["js"]
---

## 虚拟 DOM

    1、对真实 DOM 的抽象，以javascript 对象作为基础的数，用对象的属性来描述节点，最终通过一系列的操作让这个树映射到真实的环境上
    2、 **javascript** 对象中，虚拟 dom 表现为一个 object 对象，最少包含标签名tag，属性 attrs 和子元素对象 children 三个属性

创建虚拟 dom 就是为了更好的将虚拟的节点渲染到页面视图中，虚拟 dom 对象节点和真实 dom 的属性一一对应

## diff 算法（vue）

得出一些需要修改的最小单位，减少 dom 的操作

## 为什么需要虚拟 DOM

操作真实 dom 代驾大，频繁操作容易页面卡顿

## 如何实现虚拟 DOM

vue-VNode 源码

```js
export default class VNode {
	tag: string | void;
	data: VNodeData | void;
	children: ?Array<VNode>;
	text: string | void;
	elm: Node | void;
	ns: string | void;
	context: Component | void; // rendered in this component's scope
	functionalContext: Component | void; // only for functional component root nodes
	key: string | number | void;
	componentOptions: VNodeComponentOptions | void;
	componentInstance: Component | void; // component instance
	parent: VNode | void; // component placeholder node
	raw: boolean; // contains raw HTML? (server only)
	isStatic: boolean; // hoisted static node
	isRootInsert: boolean; // necessary for enter transition check
	isComment: boolean; // empty comment placeholder?
	isCloned: boolean; // is a cloned node?
	isOnce: boolean; // is a v-once node?

	constructor(
		tag?: string,
		data?: VNodeData,
		children?: ?Array<VNode>,
		text?: string,
		elm?: Node,
		context?: Component,
		componentOptions?: VNodeComponentOptions
	) {
		/*当前节点的标签名*/
		this.tag = tag;
		/*当前节点对应的对象，包含了具体的一些数据信息，是一个VNodeData类型，可以参考VNodeData类型中的数据信息*/
		this.data = data;
		/*当前节点的子节点，是一个数组*/
		this.children = children;
		/*当前节点的文本*/
		this.text = text;
		/*当前虚拟节点对应的真实dom节点*/
		this.elm = elm;
		/*当前节点的名字空间*/
		this.ns = undefined;
		/*编译作用域*/
		this.context = context;
		/*函数化组件作用域*/
		this.functionalContext = undefined;
		/*节点的key属性，被当作节点的标志，用以优化*/
		this.key = data && data.key;
		/*组件的option选项*/
		this.componentOptions = componentOptions;
		/*当前节点对应的组件的实例*/
		this.componentInstance = undefined;
		/*当前节点的父节点*/
		this.parent = undefined;
		/*简而言之就是是否为原生HTML或只是普通文本，innerHTML的时候为true，textContent的时候为false*/
		this.raw = false;
		/*静态节点标志*/
		this.isStatic = false;
		/*是否作为跟节点插入*/
		this.isRootInsert = true;
		/*是否为注释节点*/
		this.isComment = false;
		/*是否为克隆节点*/
		this.isCloned = false;
		/*是否有v-once指令*/
		this.isOnce = false;
	}

	// DEPRECATED: alias for componentInstance for backwards compat.
	/* istanbul ignore next https://github.com/answershuto/learnVue*/
	get child(): Component | void {
		return this.componentInstance;
	}
}
```

- 所有对象的 context 指向 Vue 实例
- el 属性指向对应的真实 dom

**vue 通过 createElement 生成 VNode**

### 源码 create-element.js

```js
export function createElement(
	context: Component,
	tag: any,
	data: any,
	children: any,
	normalizationType: any,
	alwaysNormalize: boolean
): VNode | Array<VNode> {
	if (Array.isArray(data) || isPrimitive(data)) {
		normalizationType = children;
		children = data;
		data = undefined;
	}
	if (isTrue(alwaysNormalize)) {
		normalizationType = ALWAYS_NORMALIZE;
	}
	return _createElement(context, tag, data, children, normalizationType);
}
```

`createElement`方法是对`_createElement`方法的封装，对参数的传入进行判断

\_createElement:

```js
export function _createElement(
    context: Component,
    tag?: string | Class<Component> | Function | Object,
    data?: VNodeData,
    children?: any,
    normalizationType?: number
): VNode | Array<VNode> {
    if (isDef(data) && isDef((data: any).__ob__)) {
        process.env.NODE_ENV !== 'production' && warn(
            `Avoid using observed data object as vnode data: ${JSON.stringify(data)}\n` +
            'Always create fresh vnode data objects in each render!',
            context
        )
        return createEmptyVNode()
    }
    // object syntax in v-bind
    if (isDef(data) && isDef(data.is)) {
        tag = data.is
    }
    if (!tag) {
        // in case of component :is set to falsy value
        return createEmptyVNode()
    }
    ...
    // support single function children as default scoped slot
    if (Array.isArray(children) &&
        typeof children[0] === 'function'
    ) {
        data = data || {}
        data.scopedSlots = { default: children[0] }
        children.length = 0
    }
    if (normalizationType === ALWAYS_NORMALIZE) {
        children = normalizeChildren(children)
    } else if ( === SIMPLE_NORMALIZE) {
        children = simpleNormalizeChildren(children)
    }
	// 创建VNode
    ...
}
```

`_createElement`接受的 5 个参数

`context`:表示 Vnode 的上下环境，Component 类型

`tag`：标签，可以是一个字符串

`data`：表示 VNode 数据，VNodeData 类型

`children`：表示当前 VNode 的子节点

`normalizationType`：表示子节点规范的类型，

---

## 小结

`createElement` 创建 `VNode` 过程，每个 `VNode` 有 children，children 每个元素也是一个 `VNode` ，形成一个虚拟数的结果，描述真实 `DOM` 树结构
