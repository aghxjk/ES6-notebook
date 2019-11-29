## 01. let 命令
```text
1. let命令类似于var用来声明变量
	1.1 let声明的变量只在代码块{}内有效;
	1.2 let在for (let i = 0; i < 3; i++){}中使用,设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域;
	1.3 let不存在变量提升,即：先声明,再使用;
	1.4 暂时性死区(temporal dead zone，简称 TDZ): 块内变量会覆盖全局变量。
		a) 示例: 下面代码会报错
			var tmp = 123;
			
			if (true) {
			  tmp = 'abc'; // ReferenceError (先使用)
			  let tmp;                       (后声明)
			}
		b) typeof不再是一个百分之百安全的操作
			typeof x; // ReferenceError
			let x;    // 如果没有给let变量声明,上面的typeof反而不会报错
	1.5 let不允许重复声明同一个变量
		例如:
			function func() { // 报错
			  let a = 10;
			  var a = 1;
			}
			
			function func(arg) {
			  let arg;
			}
			func() // 报错

2. 块级作用域
	2.1 ES5没块级作用域,弊端如下:
		a) 内层变量可能会覆盖外层变量
			var tmp = new Date();
			
			function f() {
			  console.log(tmp);
			  if (false) {
			    var tmp = 'hello world';
			  }
			}
			
			f(); // undefined
		b) 用来计数的循环变量泄露为全局变量
			var s = 'hello';
			
			for (var i = 0; i < s.length; i++) {
			  console.log(s[i]);
			}
			
			console.log(i); // 5
	
	2.2 let实际上为 JavaScript 新增了块级作用域
		function f1() {
		  let n = 5;
		  if (true) {
		    let n = 10;
		  }
		  console.log(n); // 5
		}
		
	2.3 块级作用域与函数声明
		a) ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。浏览器没有遵守这个规定。
			ES5 非法情况示例：
				// 情况一
				if (true) {
				  function f() {}
				}
				
				// 情况二
				try {
				  function f() {}
				} catch(e) {
				  // ...
				}
	
	2.4 函数声明语句的行为类似于let，在块级作用域之外不可引用
		a) 示例：
			// 块级作用域内部的函数声明语句，建议不要使用
			{
			  let a = 'secret';
			  function f() {
			    return a;
			  }
			}
			
			// 块级作用域内部，优先使用函数表达式
			{
			  let a = 'secret';
			  let f = function () {
			    return a;
			  };
			}
```
## 02. const 命令
```text
1. 基本用法
	1.1 const声明一个只读的常量。一旦声明，常量的值就不能改变;
	1.2 const的作用域与let命令相同：只在声明所在的块级作用域内有效;
	1.3 同样存在暂时性死区,只能先声明,再使用;
	
2. 本质
	2.1 const只能保证这个指针是固定的,指向的数据结构是可变的;
```
## 03. ES6 声明变量的6中方法
```text
1. ES5只有两种声明变量的方法：var命令和function命令
2. ES6 除了新增的let和const命令,还有import命令和class命令
```
## 04. 顶层对象
```text
1. 很难找到一种方法，可以在所有情况下，都取到顶层对象.
下面是两种勉强可以使用的方法:
	// 方法一
		(typeof window !== 'undefined'
		   ? window
		   : (typeof process === 'object' &&
			  typeof require === 'function' &&
			  typeof global === 'object')
			 ? global
			 : this);

	// 方法二
		var getGlobal = function () {
		  if (typeof self !== 'undefined') { return self; }
		  if (typeof window !== 'undefined') { return window; }
		  if (typeof global !== 'undefined') { return global; }
		  throw new Error('unable to locate global object');
		};
```