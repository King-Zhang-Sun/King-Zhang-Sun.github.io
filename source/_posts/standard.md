---
title: 开发规范
date: 2024-06-30 14:39:16
password: 123456
abstract: 请输入密码以查看完整内容
message: 请输入密码以查看完整内容
categories:
  - 其他
tags:
  - Standard
---

## 命名要求

- `src文件夹`下，除了`src/pages/*`和`src/components/*`这两个文件夹外，所有文件夹和文件的命名方式都**必须**使用**小驼峰**风格

  ```
  例子：

  	src/apis/index.js
  ```

- `src/pages/*`和`src/components/*` 文件夹下，所有文件夹和文件的命名方式都**必须**使用**大驼峰**风格

  ```
  例子：

  	src/components/CommoditySelection

  	src/pages/Tool/DownloadConfig
  ```

- 模板绑定事件时函数命名**必须**使用`on`前缀

  ```
  // 增，删，改，查，保存，提交，重置等功能性的
  // onCreate, onDelete, onUpdate, onFind/onQuery, onSave, onSubmit, onReset
  例子(vue)：

  	@click='onSave'

  	methods: { onSave () { ...... } }

  例子(React)：
    onClick='onSave'

    const onSave = () => {
      ......
    }
  ```

- 事件处理时函数命名**必须**使用`handle`前缀

  ```
  例子(vue)：

    @click='handleClickName'

    methods: { handleClickName () { ...... } }

  例子(React)：

    onClick='handleClickName'

    const handleClickName = () => {
      ......
    }
  ```

## 模块化

- 模块入口文件命名格式：`ModuleName/index.* `

  例子：`CommoditySelection/index.vue`

- .vue 文件中只存放模板与样式，JS 统一分离至 JS 文件，

  ```
  例子：

  	src/pages/Urge/List.vue

  	src/pages/Urge/src/List.js
  ```

- `src/pages/{主模块}/{子模块}/{资源文件}` 3 级目录 的方式，参考原客道 CRM “views”规范，

  名词：

  ​ 主模块 === 一级菜单

  ​ 子模块 === 一级菜单下的所有功能页面

  ​ 资源文件 === 页面的图片、CSS 文件、JS 文件等

  ```
  例子：

      src/pages/Tool/WxAccount

      src/pages/Tool/WxTemplate

      src/pages/Tool/WxTemplate/src/NsTableWxTemplate.js
  ```

## .vue 文件使用

- .vue 文件中的私有 style 标签都**必须**添加 scoped 属性，以防止与全局样式冲突

  例子：`<style scoped>`

- .vue 文件中，不可以有空 style 、template、script 标签存在

## 数据获取

- 系统里的数据获取，如无特殊要求都必须使用 `this.$http`这一个方法

- 未使用`this.$http`获取数据时，URL 必须统一添加 `API_ROOT`参数

  例子：`api.API_ROOT + '/myApiUrl'`

## import 使用规范

- **必须**使用 `import XXX from 'XXX'` ，避免使用`import {XXX} from 'XXX' `

  因使用 {XXX} 方式无法实现按需导入 , 会把该文件的所有对象进行导入。如特殊影响需要，可使用{XXX}，不过需严格控制。

## 系统文案规范

- 数据正在加载中：`$t('prompt.loading') === 正在加载...`

- 选择提示 ：`$t('prompt.select') === 请选择`

- 查询不到数据：`$t('prompt.noData') === 暂无数据`

- 操作按钮：`新增、保存、编辑、删除、确定`

  注：

  1.  保存数据类，统一使用 “保存” 确认操作或步骤类 “确定”
  2.  数据修改弹窗操作按钮：取消、保存
  3.  信息展示弹窗操作按钮：关闭
  4.  有保存状态不同时：取消、保存并提交审核、保存
  5.  页面类型 操作按钮为：保存、重置

- 表单内提示文案：“请选择 XXX”、"请输入 XXX" 以能提示用户输入什么类型、多长的数据为准

- 文案与动态值之间要有空格

  正确：您已录入 0 个字(含签名)，将被做为 0 条短信发送，每条按 66 字计价。

  错误：您已录入 0 个字(含签名)，将被做为 0 条短信发送，每条按 66 字计价。

- 提示文案后面统一增加句号（。）

- 按钮文案：

  1. 是将数据保存至服务器的 “保存”
  2. 流程性的、与服务器操作无关的操作 “确定”

- [i18n 官方文档参考链接](https://kazupon.github.io/vue-i18n/zh/)

### 在模板中使用示例

```vue
// $t(''operating.search) // $t(''operating.reset)

<ns-button type="primary" @click="onSearch">{{$t('operating.search')}}</ns-button>
<ns-button @click="onResetSearch">{{$t('operating.reset')}}</ns-button>
```

### 在脚本中使用示例

```javascript
// this.$t('operating.edit')

var operateButtons = [
  {
    func: function () {},
    icon: '',
    name: this.$t('operating.edit'),
    auth: ``,
    visible: '',
  },
];
```

## 弹窗配置规范

- 数据操作型弹窗，点击弹窗之外单号**不**关闭弹窗。

- 信息展示型弹窗，点击弹窗之外单号、键盘 ESC 键**要**关闭弹窗。

  注：配置方法见[弹窗 API](http://192.168.1.24:8002/#/zh-CN/component/dialog)

- 弹窗按钮顺序由左至右，权重依次加深。

  如：取消、上一步、下一步、确定

## 项目配置文件修改

除`src/**/*`文件夹及其下的文件，**其余所有**的文件和文件夹的修改**都需要**经过项目负责人的同意，不可私自修改和提交代码。

## 对象的挂载与开发

- 系统中对象分三个级别

  1.  全局：`window`、`vue`、`vue.store`等

      此类全局共享，使用时无需再自行引入，增加或修改时需经过项目负责人同意；

  2.  公共：`src/utils`、`src/mixins`、`src/components`等

      此类共享工具，组件类，使用时需自行引入，增加或修改时需经过项目负责人同意；

  3.  私有：`src/pages/XX/components`、`src/pages/XX/mixins`、`src/pages/XX/utils`等

      此类当前模块工具、组件类，使用时需自行引入，可自行增加或修改。

- JavaScript 原生方法扩展、重写，需经过项目负责人同意。

## 图标使用规范

### 图标添加&更新

#### svg 图标

① 从相关人员或部门处（前端、设计或产品）获取"svg"格式的图标文件；

② 将文件放到项目的`src/icons/`目录下对应[风格](#图标风格)的文件夹内；

③ 若为**添加图标**，则文件名与已有文件名不重复，并且**必须**与图标库中图标名保持一致；

​ 若为**图标更新**，则保持同名，并直接**覆盖**旧图标；

④ 执行`npm run bootstrap`。

::: warning 图标更新

1. 更新代码后，若图标有更新，则必须执行`npm run bootstrap`；
2. 若项目正在运行，则必须停止项目才能执行上方代码。

:::

#### 字体图标

`src/theme/[主题名]/icon.pcss`入口文件按顺序依次引入：图标类名规则`iconfont.css`和[web 字体定义](https://www.jianshu.com/p/b41c1b5b3799) 不可在此入口文件编写其他样式， 一个系统可引入多套图标。

1. 添加或修改图标，请参考[阿里巴巴矢量库(Iconfont)引入方法](https://www.jianshu.com/p/2ae4de406ca8)，将下载后的`iconfont.css`文件的[web 字体定义](https://www.jianshu.com/p/b41c1b5b3799)移至入口文件中声明。
2. 如需从其他项目拷贝图标库，则拷贝[阿里巴巴矢量库(Iconfont)引入方法](https://www.jianshu.com/p/2ae4de406ca8)中涉及的图标文件即可。 ::: tip 子项目的字体图标

[子项目](#子项目)引用 ecrp-ecrm 图标库: .woff 和.ttf 文件为系统必须引用的[web 字体定义](https://www.jianshu.com/p/b41c1b5b3799)。覆盖规则为声明顺序后者权限更高。

:::

::: warning 系统引入多套图标

1. 当系统中引入多套图标时，需要保证[web 字体定义](https://www.jianshu.com/p/b41c1b5b3799)的唯一性，否则多套图标中只有后者的字体图标定义有效。
2. 且多套图标导入图标类名规则时，需统一放在文件最上方，否则系统报错。

:::

```css
@import '@nascent/ecrp-ecrm/src/theme/[主题名]/fonts/iconfont.css';
@import './fonts/iconfont.css';
@font-face {
  font-family: 'iconfont';
  src: url('../../../node_modules/@nascent/ecrp-ecrm/src/theme/[主题名]/fonts/iconfont.woff') format('woff'),
    url('../../../node_modules/@nascent/ecrp-ecrm/src/theme/[主题名]/fonts/iconfont.ttf') format('truetype');
}
@font-face {
  font-family: 'iconfont-private';
  src: url('./fonts/iconfont.woff') format('woff'), url('./fonts/iconfont.ttf') format('truetype');
}
```

### 图标使用

::: tip 支持 Ant Design 图标

目前支持直接使用所有[Ant Design](https://ant.design/components/icon-cn/) 提供的图标

:::

具体参数请查看[Icon 文档](http://192.168.1.24:8002/#/zh-CN/component/icon)

```vue
// 假设图标文件名为test.svg // 使用方式
<Icon type="test" />
```

::: warning 图标样式调整

若图标颜色需要更改，则需再进行以下步骤：

1. 编辑对应的 svg 文件，删除文件内所有的 fill 属性；
2. 给图标添加样式，根据页面实际情况选择直接给 Icon 添加类名，或在 Icon 外包裹一层带了样式的 span。

:::

### 图标风格

图标按照风格分为以下三种：

| 风格     | 风格名   | 对应文件夹名 |
| :------- | -------- | ------------ |
| 线框风格 | filled   | fill         |
| 实底风格 | outlined | outline      |
| 双色风格 | twoTone  | twoTone      |

在`src/icons/`目录下按风格分为三个文件夹（若文件夹不存在则自行创建）。

※ 风格划分可参考[Ant Design](https://ant.design/components/icon-cn/) 的图标划分方式

### 图标不完整问题

- 若出现图标展示不完整的情况，原因为提供的图标不符合要求（图标超出可视区或不居中，详细规则可参考阿里图标库[图标绘制规则](https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d1c73b2de)）；
- 解决方法
  1. 在图标库中编辑对应图标，适当缩放或移动图标位置；
  2. [更新图标](#图标添加&更新)

## 表单使用规范

### 介绍

表单[el-form](http://192.168.1.24:8002/#/zh-CN/component/form)分为两种类型：[行内表单](#行内表单)与[水平表单](#水平表单)，表单中涉及输入框、选择框、单选框、多选框等控件。各个控件的排版宽度分为两种模式：[自适应宽度](#自适应宽度)与[固定宽度](#固定宽度)

### 名词解析

#### 表单标签与表单域

| 如下图所示, 表单标签可为空值 |
| :--------------------------- |

#### 行内表单

| 它的所有元素是内联的，所有的表单标签与表单域都是并排的。[行内表单配置方法 :inline="true"](http://192.168.1.24:8002/#/zh-CN/component/form) |
| :-- |

#### 水平表单

| 表单标签与表单域是水平并排的，表单域内容可以多行多列。[水平表单 API](http://192.168.1.24:8002/#/zh-CN/component/form) |
| :-- |

#### 自适应宽度

控件的宽度为父级容器宽度的 100%。系统分栏布局详见[el-row](http://192.168.1.24:8002/#/zh-CN/component/layout)

#### 固定宽度

指定宽度是多少，控件的宽度就为多少。系统宽度梯度详见[el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid)

### 行内表单排版分类

| 序号 | 组件宽度 | 描述 | 示例 | API |
| --- | :-- | :-- | :-- | :-- |
| 001 | 自适应宽度 | 表单中各个组件的宽度为父级容器宽度的 100% | [组件自适应宽度](#组件自适应宽度) | [el-form :inline="true"](http://192.168.1.24:8002/#/zh-CN/component/form) |
| 002 | 固定宽度 | 表单中各个组件宽度使用自定义的值 | [组件固定宽度](#组件固定宽度) | [el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid) |

### 水平表单排版分类

| 序号 | 表单域行数 | 表单域列数 | 组件宽度 | 描述 | 示例 | API |
| --- | :-- | :-- | :-- | :-- | :-- | :-- |
| 001 | 单行 | 单列 | 自适应宽度 | 表单中各个组件宽度自适应撑满父容器的宽度 | [单行单列组件自适应宽度](#单行单列组件自适应宽度) | [el-form](http://192.168.1.24:8002/#/zh-CN/component/form) |
| 002 | 单行 | 单列 | 固定宽度 | 自定义组件宽度固定值 | [单行单列组件固定宽度](#单行单列组件固定宽度) | [el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid) |
| 003 | 单行 | 多列 | 自适应宽度 | 每列组件按配置自适应宽度 | [单行多列组件自适应宽度](#单行多列组件自适应宽度) | [el-col](http://192.168.1.24:8002/#/zh-CN/component/layout) |
| 004 | 单行 | 多列 | 固定宽度 | 每列组件按配置固定宽度 | [单行多列组件固定宽度](#单行多列组件固定宽度) | [el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid) |
| 005 | 多行 | 单列 | 自适应宽度 | 表单中各个组件宽度自适应撑满父容器的宽度 | [多行单列组件自适应宽度](#多行单列组件自适应宽度) | [el-form](http://192.168.1.24:8002/#/zh-CN/component/form) |
| 006 | 多行 | 单列 | 固定宽度 | 自定义组件宽度固定值 | [多行单列组件固定宽度](#多行单列组件固定宽度) | [el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid) |
| 007 | 多行 | 多列 | 自适应宽度 | 每列组件按配置自适应宽度 | [多行多列组件自适应宽度](#多行多列组件自适应宽度) | [el-col](http://192.168.1.24:8002/#/zh-CN/component/layout) |
| 008 | 多行 | 多列 | 固定宽度 | 每列组件按配置固定宽度 | [多行多列组件固定宽度](#多行多列组件固定宽度) | [el-form-grid](http://192.168.1.24:8002/#/zh-CN/component/form-grid) |

### 表单排版代码示例

#### 行内表单

##### 组件自适应宽度

<!-- ![img](./assets/img/inline-auto.png) -->

```vue
<el-form :inline="true">
  <el-form-item label="审批人"><el-input></el-input></el-form-item>
  <el-form-item label="单号"><el-input></el-input></el-form-item>
</el-form>
```

##### 组件固定宽度

<!-- ![img](./assets/img/inline-fix.png) -->

```vue
<el-form :inline="true">
  <el-form-item label="审批人">
     <el-form-grid size="md"><el-input></el-input></el-form-grid>
  </el-form-item>
  <el-form-item label="单号">
     <el-form-grid size="md"><el-input></el-input></el-form-grid>
  </el-form-item>
</el-form>
```

#### 水平表单

##### 单行单列组件自适应宽度

<!-- ![img](./assets/img/11-auto.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人"><el-input></el-input></el-form-item>
</el-form>
```

##### 单行单列组件固定宽度

<!-- ![img](./assets/img/11-fix.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-form-grid size="md"><el-input></el-input></el-form-grid>
 </el-form-item>
</el-form>
```

##### 单行多列组件自适应宽度

<!-- ![img](./assets/img/1n-auto.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-col :span="12"><el-input></el-input></el-col>
    <el-col :span="12"><el-input></el-input></el-col>
 </el-form-item>
</el-form>
```

##### 单行多列组件固定宽度

<!-- ![img](./assets/img/1n-fix.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-form-grid size="md"><el-input></el-input></el-form-grid>
    <el-form-grid size="md"><el-input></el-input></el-form-grid>
 </el-form-item>
</el-form>
```

##### 多行单列组件自适应宽度

<!-- ![img](./assets/img/21-auto.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-form-item><el-input></el-input></el-form-item>
    <el-form-item><el-input></el-input></el-form-item>
 </el-form-item>
</el-form>
```

##### 多行单列组件固定宽度

<!-- ![img](./assets/img/21-fix.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-form-grid size="md" block><el-input></el-input></el-form-grid>
    <el-form-grid size="md" block><el-input></el-input></el-form-grid>
 </el-form-item>
</el-form>
```

##### 多行多列组件自适应宽度

<!-- ![img](./assets/img/2n-auto.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
      <el-form-item>
            <el-col :span="12"><el-input></el-input></el-col>
            <el-col :span="12"><el-input></el-input></el-col>
      </el-form-item>
      <el-form-item>
           <el-col :span="12"><el-input></el-input></el-col>
           <el-col :span="12"><el-input></el-input></el-col>
      </el-form-item>
 </el-form-item>
</el-form>
```

##### 多行多列组件固定宽度

<!-- ![img](./assets/img/2n-fix.png) -->

```vue
<el-form label-width="80px">
 <el-form-item label="审批人">
    <el-form-item>
      <el-form-grid size="md"><el-input></el-input></el-form-grid>
      <el-form-grid size="md"><el-input></el-input></el-form-grid>
    </el-form-item>
    <el-form-item>
        <el-form-grid size="md"><el-input></el-input></el-form-grid>
        <el-form-grid size="md"><el-input></el-input></el-form-grid>
    </el-form-item>
 </el-form-item>
</el-form>
```

## 样式使用规范

### 目录规则

- 全局样式目录：`src/theme/`

- 系统风格样式包：

  ​ 默认样式包`src/theme/default`

  ​ 小尺寸样式包`src/theme/[主题名]`

- 组件样式：`src/theme/default/NuiJs`

- 公共布局：`src/theme/default/public`

- 系统样式变量：`src/theme/default/variables.pcss`

### 编写规则

1. 系统样式变量：包含基础变量、逻辑层变量（见下方的[开发要求](#开发要求)中详解基础变量与逻辑层变量）

2. 样式编写：使用[BEM 规则](https://www.w3cplus.com/css/bem-definitions.html)

3. 页面样式：在 style 标签上添加 scoped，结合页面命名空间`@component-namespace`实现样式私有化。

4. 页面变量：在当前页面 style 中定义，不可添加至系统样式变量文件

```vue
<style scoped>
:root {
  --list-item-selected-bg: #eee; /* 页面变量 */
}
</style>
```

### 引用方式

- 全局样式引用

  1.  配置入口：`src/main.js`

  2.  路径引用：使用相对路径

  3.  内容包含： 公共布局、组件样式

- 变量引用

  1.  逻辑层变量：引用基础变量，或根据应用扩展自定义逻辑变量

      ```css
      :root {
        --theme-base-border-color-primary: #dcdfe6; /* 基础变量 */
        --default-input-border: var(--theme-base-border-color-primary); /* 逻辑层变量引用基础变量 */
        --default-th-vertical-padding: 8px; /* 逻辑层自定义变量 */
      }
      ```

  2.  样式定义：无论全局样式还是页面私有样式均引用逻辑层变量，禁止引用基础变量

- 路径引用

  1.  页面中引用系统样式变量：

  ​ 使用别名引入`@import "@theme/variables.pcss";`

  ​ 定义别名的文件在项目根目录下的`postcss.config.js `

  2.  图片路径引用：public 目录下的图片使用绝对路径引用，非 public 目录下的图片使用相对路径引用

### 开发要求

1. 项目样式分为两部分：全局样式和页面样式（见第 3 点和第 4 点）。

2. 样式变量分为两部分：基础变量和逻辑层变量。

   a. 基础变量：包含项目公共的基础色系梯度（如文字颜色、页面背景色、组件边框色、禁用文字/背景色）、间距梯度、按钮圆角等

   b. 逻辑层变量：包含项目公共布局（如导航、菜单）和组件涉及的颜色、间距、字体大小等变量。

   ```css
   :root{
     /* ——基础变量—— */

     /* 基础色系梯度 */
     --theme-color-primary:                         #0091FA;
     --theme-color-primary-light:                   #41a2e8;
     --theme-color-primary-bg:                      #f6fbff;  /* 系统主色背景色 */
     --theme-color-success:                         #67c23a;
     --theme-color-success-bg:                      #f6fdf9;
     --theme-color-warning:                         #e6a23c;
     --theme-color-danger:                          #f56c6c;
     --theme-color-error:                           #f56c6c;

     --theme-bg-color-base:                         #F3F4F4; /* 系统通用背景色 */
     --theme-bg-color-primary:                      #f5f7fa;
     --theme-color-white:                           #fff;  /* 系统背景下的反白色 */

     /* 文本及边框颜色 */
     --theme-font-color-primary:                    #33393E; /* 主要文字 */
     --theme-font-color-regular:                    #606266; /* 常规文字 */
     --theme-font-color-secondary:                  #909399; /* 次要文字 */
     --theme-font-color-tips:                       #c0c4cc; /* 提示文字 */
     --theme-base-border-color-primary:             #dcdfe6; /* 系统通用边框色 */

     ……

    /* ——逻辑层变量—— */

     /* input */
    --default-input-suffix-bg:                     var(--theme-bg-color-base); /* 引用基础变量 */
    --default-input-suffix-color:                  var(--theme-bg-color-primary);
    --default-input-suffix-line-height:            var(--default-font-line-height);
    --default-input-radius:                        var(--default-radius-mini);
    --default-input-border:                        var(--theme-base-border-color-primary);

    /* table */
    --default-table-strip-bg:                      var(--theme-bg-color-base);
    --default-table-tr-hover:                      var(--theme-color-primary-light);
    --default-table-head-border:                   var(--theme-base-border-color-primary);
    --default-th-vertical-padding:                 8px; /* 逻辑层自定义变量 */
    --default-td-vertical-padding:                 6px;

   ……

     }
   ```

3. 全局样式：包含公共布局和组件样式。全局样式只能由前端开发人员进行增删改，需检查其影响范围并测试全面。

   a. 引用入口：路径`src/main.js`，使用相对路径引用全局样式（公共布局、组件样式）

   ```javascript
   /* 引入公共布局 */
   import './style/small/index.scss';
   /* 引入组件样式 */
   import './style/small/NuiJs/index.scss';
   ```

   b. 公共布局：路径`src/theme/[主题名]/index.p`，依次引入系统样式变量、全局初始化样式和页面布局

   ```css
   /* .scss文件的变量覆盖原则：上方代码覆盖下方代码 */

   /* 系统样式变量 */
   @import './variables.pcss';
   /* 全局初始化样式 */
   @import './global.pcss';
   /* 页面布局 */
   @import './public/page.pcss';
   ```

   c. 组件样式：路径`src/theme/NuiJs/index.scss`，包含 nui 2.0 的各个组件样式定义

   ```scss
   /* .scss文件的变量覆盖原则：上方代码覆盖下方代码 */

   /* element-ui组件库样式主变量文件修改版 */
   @import 'var';

   /* 改变 icon 字体路径变量，必需 */
   $--font-path: '~nui-v2/lib/theme-chalk/fonts';

   /* element-ui组件库样式原文件 */
   @import '~nui-v2/packages/theme-chalk/src/index';

   /* 复写 element-ui组件库样式 */
   @import './components/index.pcss';
   ```

4. 页面样式：

   a. 在 style 标签上添加样式私有化标识`scoped`, 结合页面命名空间`@component-namespace`实现当前样式私有化，避免污染全局样式。

   b. 使用“别名方式”引用系统样式变量 `@import "@theme/variables.pcss";`

   c. 如需添加私有变量，则在文档根元素作用域[:root](http://www.w3school.com.cn/cssref/selector_root.asp)中自定义页面变量。

   d. 使用[BEM 规则](https://www.w3cplus.com/css/bem-definitions.html)编写。

   ```vue
   <template>
     <ul class="subdivision-list">
       <li class="subdivision-list__item">普通列表行数据</li>
       <li class="subdivision-list__item subdivision-list__item--selected">选中列表行数据</li>
     </ul>
   </template>

   <style scoped>
   /* 引入系统样式变量 */
   @import '@theme/variables.pcss';

   /* 自定义变量 */
   :root {
     --list-item-selected-bg: #eee;
   }

   /* 页面样式编写 */
   @component-namespace subdivision {
     @b list {
       border: 1px solid #eee;
       @e item {
         /* 使用系统样式变量 */
         color: var(--theme-font-color-regular) @m selected {
           /* 使用自定义变量 */
           background: var(--list-item-selected-bg);
         }
       }
     }
   }
   </style>
   ```

5. 项目因开发需求，可能存在多个项目样式包，方便项目风格切换。

   a. 默认样式包

   ```
   src/theme/default
   ```

   b. 小尺寸样式包

   ```
   src/theme/[主题名]
   ```
