# 1 前言 

首先我们公司的前端开发在代码风格上非常的混乱，整体来看即使是在具备脚手架项目的情况之下都存在结构混乱的问题。本来以为在一个统一的参考之下，各位的代码风格会逐步的趋于统一，但是实际情况是依旧是各自为战，甚至愈演愈烈。一方面代码复杂度过高导致交叉协同工作开展困难；另一方面混乱的代码给新员工造成了极大的上手难度，我简直是痛心疾首。所以最后不得已约定一套全公司前端开发统一的规范。

该代码规范为最高优先服从的规范，一旦发布之后全体前端开发必须服从，任何异议可以直接向我提出，确定合理的情况下我们可以提交修订版本，但是已经提交的规范哪怕你不认同也必须执行。

目前这些规范一部分来源是**王子荣**提供的`JS`编码规范，一部分来源是VUE官方的风格指南，还有一部分是我定义的基础规范。

# 2 核心组件

 <p align="left">
    <img src="https://img.shields.io/badge/VUE-3.1.1-green.svg" />
   	<img src="https://img.shields.io/badge/axios-0.18.0-blue.svg" />
     <img src="https://img.shields.io/badge/babel-6.26.0-red.svg" />
     <img src="https://img.shields.io/badge/ElementUI-2.11.1-green.svg" />
     <img src="https://img.shields.io/badge/UView-1.8.4-blue.svg" />
 </p>  

这里的组件一方面是我们管理后台的前端项目之中所用的基础组件。曾经我们的开发人员居然在请求数据明明有`Axios`的情况下居然还去使用`JQuery`。后续的开发过程之中除非与我或者秦佳旺单独沟通过，任何情况下禁止使用其他类库去实现这些预置库能够实现的功能。组件部分我们不强制要求版本一致，但是在进行任何依赖版本变更的时候一定要想清楚历史组件的兼容性会不会出问题。

我们前端开发无论是小程序、H5、APP还是其他什么鬼都一样，只要是你在用JS编写界面那么统一去使用**VUE**，禁止私自定义前端框架并且项目里面使用。**UI**框架基于`UniAPP`开发的项目统一使用`UViewUI`、其他项目统一去使用`ElementUI`。

# 3 JS通用编码规范

## 3.1 代码检查

无论任何项目管他是三维、H5、小程序、APP还是其他什么东西只要是在用`JavaScript`或者`TypeScript`编写都必须通过`ESLint`检查，我们公司的所有脚手架代码都集成与预置了`ESLint`检查插件，禁止任何人对其进行规则进行自定义或者关闭检查。目前通用的`ESLint`配置如下：

```javascript
module.exports = {
  root: true,
  env: {
    node: true
  },
  'extends': [
    "plugin:vue/essential"
  ],
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off'
  },
  parserOptions: {
    parser: 'babel-eslint'
  }
}

```

目前我们允许在项目开发过程之中（开发环境与测试环境）关闭`no-console`与`no-debugger`规则，但是生产环境必须开启。一旦后续再生产环境之中看到`console.log`代码那就是罪大恶极。

在编译之前应当通过`ESLint`工具进行代码检查，目前我们的模板项目之中已经集成了检查命令但是我看好像都没有用，后续必须开启并且实际使用：

```javascript
// 这是之后编译所使用的package.json里面的脚本配置

"scripts": {
    "pre": "yarn --registry https://registry.npm.taobao.org || npm install --registry https://registry.npm.taobao.org ",
    "dev": "npm run lint && vue-cli-service serve",
    "build": "npm run lint && vue-cli-service build",
    "build:online": "npm run lint && vue-cli-service build --mode online",
    "lint": "vue-cli-service lint"
}
```

## 3.2 命名规范

1. **命名规则如下**

| 结构       | 规则   | 例子                 |
| :--------- | :----- | :------------------- |
| 类         | 驼峰式 | ModuleClass()        |
| 公有方法   | 混合式 | getPosition()        |
| 变量或参数 | 混合式 | frameStyle           |
| 常量       | 大写式 | DEFAULT_FRAME_LAYOUT |

2. **特殊命名约定如下：**

  - 前面加 `is` 的变量应该为布尔值，也可使用 `can`、`has`、`should`、`be`
  - 前面加 `str` 的变量名应该为字符串
  - 前面加 `arr` 的变量应该为数组
  - 前面加 `num` 或 `count` 的变量名应该为数字
  - `obj` 作为局部变量或参数，表示 `Object`
  - `ele` 作为局部变量或参数，表示 `Element`
  - `evt` 作为局部变量或参数，表示 `event`
  - `err` 作为局部变量或参数，表示 `error`
  - 处理错误的变量或方法，后面跟着 `Error` 提高辨识度
  - 初始化用的函数，必须使用 `init` 开头，如果一个页面只有初始化可以单独使用 `init()`

## 3.3 编码规范

1. **二元运算符两侧必须有一个空格，一元运算符与操作对象之间不允许有空格**

   ```javascript
   let a = !arr.length;
   a++;
   a = b + c;
   ```

2. **禁止使用动态属性，所有属性必须预先声明或定义**

   ```javascript
   // Bad
   let data = {strPatamOne : "some string"};
   /* some codes */
   data.strPatamOne = "some string one";
   data.strPatamTwo = "some string two";
   
   // Good
   let data = {strPatamOne : "default string", strPatamTwo : undefined};
   /* some codes */
   data.strPatamOne = "some string one";
   data.strPatamTwo = "some string two";
   ```

3. **用作代码块起始的左花括号 `{` 前必须有一个空格**

   ```javascript
   // Bad
   if (condition){
   }
   while (condition){}
   function funcName(){}
   
   // Good
   if (condition) {
   }
   while (condition) {}
   function funcName() {}
   ```

4. **`if / else / for / while / function / switch / do / try / catch / finally` 关键字后，必须有一个空格**

   ```javascript
   // Bad
   if(condition) {
   }
   while(condition) {}
   switch(condition) {
   }
   
   // Good
   if (condition) {
   };
   while (condition) {};
   switch (condition) {
   };
   ```

5. **对象创建时，属性中的 `:` 之后必须要有空格，`:` 之前不允许有空格**

   ```javascript
   // Bad
   var obj = {
     a:1,
     b :2,
     c : 3
   };
   
   // Good
   var obj = {
     a: 1,
     b: 2,
     c: 3 // es6语法可使用尾逗号
   };
   ```

6. **函数声明、具名函数表达式、函数调用中，函数名和 `(` 之间不允许有空格**

   ```javascript
   // Bad
   function funcName () {}
   let funcName = function funcName () {};
   funcName ();
   
   // Good
   function funcName() {}
   let funcName = function funcName() {};
   funcName();
   ```

7. **`,` 和 `;` 前面不允许有空格，如果不位于行尾，`,` 和 `;` 后必须跟一个空格**

   ```javascript
   // Bad
   funcName(a ,b) ;
   
   // Good
   funcName(a, b);
   ```

8. **在函数调用，函数声明、括号表达式、属性访问、`if / for / while / switch / catch` 等语句内，`()` 和 `[]` 内紧贴括号部分不允许有空格**

   ```javascript
   // Bad
   funcName( param1, param2, param3 );
   
   save( this.list[ this.indexs[ i ] ] );
   
   isNumber && ( variable += number );
   
   if ( num > list.length ) {
   }
   
   // Good
   funcName(param1, param2, param3);
   
   save(this.list[this.indexs[i]]);
   
   isNumber && (variable += number);
   
   if (num > list.length) {
   }
   ```

9. **在等于表达式中，使用类型严格的 `===`、`!==` 判断**

   ```javascript
   // Bad
   if (age == 18) {
   }
   
   // Good
   if (age === 18) {
   }
   ```

10. **类型检测优先使用 `typeof`，对象类型检测 `instanceof`，`null`的检测使用 `x === null`**

    ```javascript
    // string
    typeof x === "string";
    // number
    typeof x === "number";
    ```

11. **所有声明类行结尾必须添加`;`不过`if / for / while / switch / catch` 语句结束无需添加。**

    ```javascript
    // Bad
    let srtName = "some string"
    
    // Good
    let srtName = "some string";
    ```

# 4 VUE文件编码规范

> 脚手架：vue/cli > 3.0
>
> Node版本：node.js > 8.9

在我们的开发中，接触最多就是：`xxx.vue` 的文件，所以我们应该制定一些规则来规范我们的代码。
按照一般的习惯组织代码，从上到下，依次是：template, script 和 style 标签。然后依次说明每个部分在编程时应该注意的问题。

## 4.1 template

> 在模版文件中，你应该遵守 HTML 的书写规范

**1、标签语义化，切忌清一色的 div 元素**。

列表可以使用 ul li，文字使用 p 标签，标题使用 h* 标签，等等。虽然 HTML5 推出了语义化的标签，但是它们还有很多兼容性问题，在不支持的浏览器导致布局错乱。所以，不建议在生产环境中使用：section，aside，header，footer，article，等 HTML5 布局标签。

**2、样式 class 的命名：BEM规范以小写字母开头，小写字母、下划线、中划线组成**。[参考这里](https://blog.csdn.net/chenmoquan/article/details/17095465)

这里千万不要使用驼峰法命名 class 的属性。以下是一些常用到的 class的名字。

> 包裹层: .xx-wrap; 列表: .xx-list; 列表项: .xx-list-item; 左边内容: .xx-left; 中间内容: .xx-middle; 右边内容: .xx-right; 某个页面: .xx-page;

我不强制要求使用BEM规范但是推荐这样做：

```
.person{} /* 块命名 */
.person__hand{} /* 块下面的元素命名 */
.person--female{} /* 块修饰符命名 */
.person--female__hand{} /* 块修饰符下的属性命名 */
.person__hand--left{}/* 块属性下的修饰符命名 */
```

**3、自定义标签：使用自闭标签的写法**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E8%87%AA%E9%97%AD%E5%90%88%E7%BB%84%E4%BB%B6-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<MyOwnerComponents/>
```

**4、多特性，分行写，提高可读性**。示例代码如下，具体原因 [看这里](https://cn.vuejs.org/v2/style-guide/#%E5%A4%9A%E4%B8%AA%E7%89%B9%E6%80%A7%E7%9A%84%E5%85%83%E7%B4%A0-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<scroll
    ref="scrollWrap"
    :data="homeData"
    :pullDownRefresh="true"
    :pullUpLoad="true"
    class="home-scroll-warp"
    @pullingDown="pullingDownGetNewData"
    @pullingUp="pullingUpGetMore"
/>
```

**5、模版中使用表达式，复杂情况使用计算属性或函数**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E6%A8%A1%E6%9D%BF%E4%B8%AD%E7%AE%80%E5%8D%95%E7%9A%84%E8%A1%A8%E8%BE%BE%E5%BC%8F-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

复杂的逻辑千万不要直接写在标签里面

```html
<!-- 在模板中 -->
{{ normalizedFullName }}
```

```javascript
// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```

**6、静态资源的引用，必须考虑非根目录情况**

这里有两种方案，推荐使用`webpack`处理静态资源，将资源文件放在`assets`文件夹下，通过`assets`进行管理：

```html
<!--使用assets文件夹下资源-->
<img src="~@/assets/img/xxx.png" />
```

如果特殊情况下不能使用`webpack`处理静态资源（可能1%都不到）需要将静态资源放在`public`文件夹下，必须这样操作：

```html
<!--使用public文件夹下资源，不经过webpack编译-->
<!--这种情况需要在vue.config.js中配置publicPath为相对路径：'./'或者特定路径：/web/-->
<!--注意：这里路径是相对路径，且不能使用./img/xxx.png，如果使用./webpack会尝试寻找对应的module-->
<img src="img/xxx.png" />
```

这样一来好处在于web服务的请求路径是非根目录如：http://domain/web/，基本是这样也不会有资源报404，极大方便了项目部署与迁移。

## 4.2 script

> 在 script 标签中，你应该遵守 ES6 的规范

**1、在 script 标签的最顶部，应该是引入文件的定义**。

禁止写一堆逻辑之后再`import`。

**2、使用vue/cli 脚手架自带的 '@' 进行组件的引入，这里不要使用相对路径。**

```javascript
import Xxxxx from '@/xxx/xxx/xxx'
```

**3、引入组件，命名使用：首字母大写的驼峰法命名**。

除开命名，统一使用 ES6 的方式引入。如果它们有依赖关系，最好体现在命名上，比如：

```javascript
import Article from 'xxx'
import ArticleDetail from 'xxx'
```

**4、组件的选项，根据官方的推荐按照以下定义**。[原文出处](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6-%E5%AE%9E%E4%BE%8B%E7%9A%84%E9%80%89%E9%A1%B9%E7%9A%84%E9%A1%BA%E5%BA%8F-%E6%8E%A8%E8%8D%90)

```javascript
<script>
  export default {
    name: 'ExampleName',  // 大写字母开头驼峰法命名。
    props: {},            // Props 定义。
    components: {},       // 组件定义。
    directives: {},       // 指令定义。
    mixins: [],           // 混入 Mixin 定义。
    data () {              // Data 定义。
      return {
        dataProps: ''     // Data 属性的每一个变量都需要在后面写注释说明用途，就像这条注释一样
      }
    },
    computed: {},         // 计算属性定义。
    created () {},         // 生命钩子函数，按照他们调用的顺序。
    mounted () {},         // 挂载到元素。
    activated () {},       // 使用 keep-alive 包裹的组件激活触发的钩子函数。
    deactivated () {},     // 使用 keep-alive 包裹的组件离开时触发的钩子函数。
    watch: {},            // 属性变化监听器。
    methods: {            // 组件方法定义。
      publicbFunction () {}  // 公共方法的定义，可以提供外面使用
      _privateFunction () {} // 私有方法，下划线定义，仅供组件内使用。多单词，注意与系统名字冲突！
    }
  }
</script>
```

**5、Data 必须是一个函数**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E6%95%B0%E6%8D%AE-%E5%BF%85%E8%A6%81)

```javascript
 data () {              // Data 定义。
     return {
     	dataProps: ''     // Data 属性的每一个变量都需要在后面写注释说明用途，就像这条注释一样
     }
 }
```

**6、Props 必须进行定义细则**。[参考这里](https://cn.vuejs.org/v2/style-guide/#Prop-%E5%AE%9A%E4%B9%89-%E5%BF%85%E8%A6%81)

```javascript
props: {
  	status: String,
	required: true,
    validator: function (value) { // 如果参数无需验证则不一定使用validator
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
}
```

**7、为 v-for 设置 key 值**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E4%B8%BA-v-for-%E8%AE%BE%E7%BD%AE%E9%94%AE%E5%80%BC-%E5%BF%85%E8%A6%81)

这里禁止直接获取必须设置一个Key之后再去获取数组内部的数据。

```html
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

**8、使用计算属性规避 v-if 和 v-for 用在一起**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81)

无论什么情况下**v-if**和 **v-for**不能放在同一个标签里面，一定要分开。

```html
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

**9、Data的属性如果是对象其下级属性必须声明定义并且赋予默认值。**[参考资料](https://cn.vuejs.org/v2/guide/reactivity.html)

禁止直接给一个对象，一方面代码维护更加繁琐，同时**data**之中声明的属性当组件数据量过大或者层级过深时是非响应式的，只能通过$set进行更新

```javascript
// 对于this.dataProps.subProps来说
// 错误的示范 
data () {
    return {
        dataProps: {}
    }
}
// 正确的写法
data () {
    return {
        dataProps: {
            subProps: 1 // 对象所有下级属性必须声明与默认值
        }
    }
}
```

## 4.3 style

> 样式部分，全部统一使用Sass，禁止直接编写原生CSS或者使用其他预处理语法，这里主要是为了统一。

**1、编码统一使用UTF-8**。

每个SASS文件的第一行必须是定义编码的 `@charset "UTF-8";`如果没定义编码，很有可能会出现跨平台兼容问题。

**2、代码顺序统一为先声明全局变量、然后导入依赖、最后完成内部声明。**

```css
@charset "UTF-8";

$base-font-size          : 12px !default;

@import "base/config";
@import "biz/page";

body {
    background: #fff;
}
```

**3、导入依赖统一。**

同一个Sass文件内，所有的`@import`必须放在同一个代码块，禁止分开。

**4、变量命名**

这里的命名与`style`的`class`命名风格一致（[参考](#_41-template)），必须采用 `@foo-bar` 形式，不允许使用 `@fooBar` 形式。

**5、如果有继承其他sass的情况，继承部分必须在开头**

```css
@charset "UTF-8";

.selector {
    @extend sameStyle;
    color: #fff;
}
```

**6、如果在SASS内使用BEM命名时，请勿使用缩写与嵌套**

```css
@charset "UTF-8";

/* 千万不要这样干 */
.aside {
    color: #fff;
    &__item {
        width: 100px;
    }
}
/* 这样才是正确的 */
.aside {
    color: #fff;
}
.aside__item {
    width: 100px;
}
```

**7、静态资源引用**

这里与模板里面的`template`相同，这里再写一次：

这里有两种方案，推荐使用`webpack`处理静态资源，将资源文件放在`assets`文件夹下，通过`assets`进行管理：

```scss
// 使用assets文件夹下资源
background: "~@/assets/img/xxx.png"
background: url("~@/assets/img/xxx.png")
```

如果特殊情况下不能使用`webpack`处理静态资源（可能1%都不到）需要将静态资源放在`public`文件夹下，必须这样操作：

```scss
// 使用public文件夹下资源，不经过webpack编译
// 这种情况需要在vue.config.js中配置publicPath为相对路径：'./'或者特定路径：/web/
// 注意：这里路径是相对路径，且不能使用./img/xxx.png，如果使用"./"的话，webpack会尝试寻找对应的module
background: "img/xxx.png"
background: url("img/xxx.png")
```

这样一来好处在于web服务的请求路径是非根目录如：http://domain/web/，基本是这样也不会有资源报404，极大方便了项目部署与迁移。

# 5 注释规范

## 5.1 哪里需要注释

写注释固然很重要，但是很多时候我们真的需要注释吗？在你写注释前，好好想一下这个问题。所以很多时候我们写的都是一些没有用的注释，还会误导人！所以，总的来说，有必要写注释的主要有：

**1、公共的函数方法、常量和重要变量的定义**。

**2、业务系统中，晦涩难懂的需求**。

**3、多步骤的流程。最好用1，2，3，4··· 标明顺序**。

**4、复杂的算法**。

其他没有用的东西直接删除，保持代码的整洁性。不要用注释的办法来设法回退，那是 git 版本管理等工具应该完成的事情。

## 5.2 注释的格式

### 5.2.1 HTML注释

在 html 中主要使用的注释方式就只有一个：```<!-- write your HTML comment! -->```

> 注意在注释的前后各有一个空格。

### 5.2.2 CSS注释

在 CSS 中，注释的方式主要有：```\* write your CSS comment! *\```

> 注意在注释的前后各有一个空格。

### 5.2.3 JS注释

在 JS 中，因为注释的东西，内容比较多，按照前面列出的功能，分别说明。

#### 行级注释

**行级注释就是指的双正斜线后面的内容，双线后面需呀放置一个空格**。

```javascript
// 正确的注释
//错误的注释，双斜线后面没有空格
```

#### 变量声明注释

**1、如果是在类似 Vue 项目的 data 属性中的变量，直接用行级样式跟在后面**。

```javascript
data () {
  return {
    rightExample: 'yes', // 注释直接写这里
    // 我是 errorExample 变量的注释，错误形式
    errorExample: 'no'
  }
}
```

**2、如果是在类，构造函数，或者常量定义中的变量，使用块级注释参考`JSDoc`规范**，[参考资料](https://jsdoc.zcopy.site/)。

```javascript
/**
* 错误码常亮定义
* @type {number}
*/
const ERR_CODE = 0
```

#### 函数声明注释

不必要在每一个函数都写注释，但是在公共函数，还是建议补全注释，让后面的人不需要重复早轮子。

**函数头注释中必须写以下内容**：

- 函数的功能
- 函数的入口参数类型
- 函数的返回值类型
- 函数的算法或者业务逻辑

```javascript
/**
 * 获取 DOM data 属性的值
 * @param {DOM} el dom对象
 * @param {String} name 需要获取属性的名字
 * @param {String, Number} val 可选，存在是表示设置属性的值
 * @returns {String}
 */
function getData (el, name, val) {
  const prefix = 'data-'
  let prop = prefix + name
  if (val) {
    el.setAttribute(prop, val)
  } else {
    return el.getAttribute(prop)
  }
}
```

# 6 代码提交约定

分支管理与版本管理安装我们之前定义的[讯视代码托管规范](http://git.saylooks.com/XDK/TF/blob/master/%E8%AE%AF%E8%A7%86%E4%BB%A3%E7%A0%81%E6%89%98%E7%AE%A1%E8%A7%84%E8%8C%83%EF%BC%88%E5%BC%BA%E5%88%B6%E6%89%A7%E8%A1%8C%EF%BC%89.md)去执行就可以了，这里需要详细说一下你在提交代码的时候`commit`里面写的东西，本来这个不算是什么大问题，但是我们前端开发人员的提交树完全看不懂在干什么，这一这里统一规定一下。

提交格式：**[关键字]:空格[内容]**

> 解释：操作关键字+英文冒号+一个空格+操作内容

以下是可以使用的操作关键字：

- **created**: 创建文件夹，JS 文件，等等
- **deleted**: 删除文件夹，JS 文件，等等
- **modified**: 修改内容或文件
- **add**: 添加文件内容，和新建不同
- **fixed**: 修复BUG，缺陷等

当然格式固定的情况下，具体的关键字可以自行补充，但是有一点必须注意，**代码提交不是每天提交一次而是每完成一个功能就进行一次提交（`commit`）**，每天应该有多次`commit`，如果说`push`到是可以每天一次。不然回退代码版本退的是个啥你知道吗？后续不要出现一个`commit`里面几百上千行代码也不知道在干嘛。

# 7 目录构建规范

```html
── project-name
   ├── public                    项目公共目录
   │   ├── favicon.ico               Favourite 图标
   │   └── index.html                页面入口 HTML 模板
   ├── src                       项目源码目录
   │   ├── main.js                   入口js文件
   │   ├── App.vue                   根组件
   │   ├── api                       把所有请求数据的接口，按照模块，写在单独的 JS 文件中
                                     目前模板项目为了自动生成分散了文件，但是后续统一这样做
   │   ├── home.js                   首页接口
   │   ├── detail.js                 详情页接口，等等
   │   │   ···
   │   ├── components            公共组件目录
   │   │   ├── confirm               弹框组件
   │   │   │   └── index.vue
   │   │   ├── scroll                局部内容滚动组件
   │   │   │   └── index.vue
   │   │   ···
   │   ├── config                全局配置，这里定义一些全局配置参数
   │   │   │── env.js                样式主文件
   │   │   │   ···
   │   ├── const                 全局常量，这里定义的常量是跨组件或跨模块使用的常量
   │   │   │── common.js             样式主文件
   │   │   │   ···
   │   ├── filters               定义一些请求数据的过滤器
   │   │   ├── index.js              默认配置
   │   │   ···
   │   ├── page                  定义一些主要页面组件
   │   │   ···
   │   ├── routes                前端路由
   │   │   └── router.js             路由配置与定义
   │   │   └── axios.js              Axios请求配置
   │   │   ···
   │   ├── store                 数据中心
   │   │   ├── modules               文件夹，里面是定义模块前端存储的一些数据项
   │   │   ├── mutations.js          同步修改 state 数据的方法定义
   │   │   ├── getters.js            获取数据的方法定义
   │   │   └── index.js              数据中心入口文件
   │   │   ···
   │   ├── styles                全局跨组件、模块定义的样式
   │   │   ···                       里面存各种sass文件
   │   ├── util                  全局通用的一些工具类
   │   │   ···                       
   │   └── views                 各个视图组件
   │       ├── module                按照功能模块区分文件夹
   │       │   └── index.vue         视图组件的VUE文件
   │       │   └── module-service.js 视图组件的接口请求service封装，后续逐步放到api文件夹中
   │       ···
   ├── .env.development              Vue 开发环境的配置
   ├── .env.production               Vue 生成环境的配置
   ├── .eslintrc.js                  Eslint 配置文件
   ├── .gitignore                    Git 忽略文件
   ├── package.json                  包依赖文件
   ├── package-lock.json             依赖包版本管理文件
   ├── README.md                     项目介绍
   ├── vue.config.js                 vue/cli 项目脚手架配置
   ···

```

**文件命名规范**

**1、所有文件（除VUE文件）和文件夹，统一使用：小写字母开头，英文中划线分隔的方式命名。必须遵守**！

```javascript
// 比如下面这种命名
module-service.js
```

**2、组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾**。[参考这里](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E5%90%8D%E4%B8%AD%E7%9A%84%E5%8D%95%E8%AF%8D%E9%A1%BA%E5%BA%8F-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

# 8 与服务端通讯

**原则：为了向上层调用的稳定，所有的请求方法都返回一个** `Promise`。当前我们的请求有一个`Axios`全局封装，所有序列化等操作全部采用全局封装实现，每一个请求`Service`都仅仅完成数据请求而已。通讯代码编写无论是使用类静态函数或者常量属性封装都可以，但是无论是常量名或者类名都应当使用`xxxxxService`进行命名。

> 不能在请求方法中添加逻辑，只做纯粹的请求。业务逻辑应该放在调用请求方法前，或者在拦截器、过滤器之中。

```javascript
import request from '@/router/axios';

const somethingService = {
    /**
     * 查询数据
     * @param {Object} payload 查询参数
     */
    getData (payload) {
      return request({
          url: '/something/data',
          method: 'get',
          params: payload
      })
    }
}

export default somethingService
```

# 9 组件封装

> 无论是视图组件还是部分自定义的公用组件除开内部基础的逻辑代码之外，有可能会存在大量的常量定义，外部逻辑。之前大量的组件封装都直接写在VUE文件的`script`里面，这样一来导致`vue`文件内部极其臃肿，可读性极差。

对任意组件封装除开基础的VUE组件属性之外的其他代码统统拿出来单独写文件，对应的结构如下：

```
└── views                 各个视图组件
    ├── module                按照功能模块区分文件夹
    	└── const.js             常量定义文件
    	└── util.js              工具类定义文件
        └── index.vue            视图组件的VUE文件
        └── module-service.js    视图组件的接口请求service封装，后续逐步放到api文件夹中
```

当然这里也不是完全定死只有这几个文件，我们也可以在内部定义一些文件夹结构出来。比如`ThreeJS`相关的功能代码都是一堆类相互依赖实现复杂逻辑这里也是一个意思，应当将这些代码从VUE文件之中抽离出来独立编写，保证结构上单一，不然谁看到懂你写的什么东西。

# 10 面向对象

> 所有三维相关项目，无论是GIS还是三维场景开发统一使用`ES6`规范下的面向对象语法与开发方式。
>
> 常规业务管理后台、小程序、APP不对对象做强制要求但是建议使用对象开发。

这一块虽然我们目前不做强制要求，但是建议所有前端人员都采用完整的面向对象思维与语法进行开发或者封装。具体的原因如下：

- 任何高复杂度的逻辑功能在没有对象的情况下开发难度很高，经常会把开发者自己都绕晕。
- 最终封装好的代码对使用者而言也是极其不友好，很难看出具体逻辑流程与结构。
- 没有对象的复杂功能结构代码可读性极差，二次开发难度巨大。
- 对任何前端开发而言一旦掌握了面向对象思维，才能进一步理解不同设计模式的巧妙，这是技术进步的一个必要条件。

原则上来说原型链和闭包设计本身是一个无奈之举，其目的本质上也是为了实现对象化操作，但是写起来大家自己想一下是不是很伤？既然ES6提供了完整面向对象语法与规范那么直接使用它不香吗？