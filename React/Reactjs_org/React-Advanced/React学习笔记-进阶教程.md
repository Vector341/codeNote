# React学习笔记-进阶教程

## Accessibility



## Code-Splitting

关于如何打包及引入代码 包括 import export



## Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.



## Forwarding Refs

在子组件中获得父组件的引用



## Higher-Order Component

类似于高阶函数, 这是一个接收一个组件参数, 返回一个组件的组件 (或者说函数, 组件本身就是一个纯函数)



## Portals

直译门户, 可以将子节点渲染到任意的 DOM 节点上 (可以在该节点的继承树外). 

Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

用于 dialog, hovercard 和 tooltips

A typical use case for portals is when a parent component has an `overflow: hidden` or `z-index` style, but you need the child to visually “break out” of its container