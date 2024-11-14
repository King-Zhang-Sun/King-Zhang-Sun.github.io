---
title: umi 使用之基础入门
date: 2024-07-02 11:52:39
categories:
- React
tags:
- umi
---

# UmiJS (Umi Max)

💻 为了更好地使用 Umi Max，我们通常选用 Ant Design Pro 模版

```ts
npx create-umi@latest
? Pick Umi App Template › - Use arrow-keys. Return to submit.
    Simple App
❯   Ant Design Pro
    Vue Simple App
```

  📖 类同于 Ant Design Pro 的 simple 模版

  ```ts
  step 1: 
    # 使用 npm
    npm i @ant-design/pro-cli -g
    pro create myapp

  step 2: 
    ? 🚀 要全量的还是一个简单的脚手架? (Use arrow keys)
    ❯ simple
      complete
  ```

## UmiJS 常用模块和API 介绍

<table style="width: 300px;margin-left: 30px">
  <thead>
    <tr>
      <th style="font-size: 20px; padding: 8px;">
        <strong>⌚️ 重点模块（插件）</strong>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="font-size: 18px; padding: 8px;"><strong>🔒 权限</strong></td>
    </tr>
    <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>📈 Antd</strong></td>
    </tr>
    <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>🌲 布局和菜单</strong></td>
    </tr>
    <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>📊 数据流</strong></td>
    </tr>
  </tbody>
</table>

<table style="width: 300px;">
  <thead>
    <tr>
      <th style="font-size: 20px; padding: 8px;">
        <strong>🔑 常用 API</strong>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>🧭 useNavigate</strong></td>
    </tr>
    <tr>
      <td style="font-size: 18px; padding: 8px;"><strong>🧧 history</strong></td>
    </tr>
    <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>🗺️ useLocation</strong></td>
    </tr>
     <tr>
      <td style=" font-size: 18px; padding: 8px;"><strong>🛞 useSearchParams</strong></td>
    </tr>
  </tbody>
</table>

## **📖 重点模块(插件)**

### **权限**

```ts
export default {
  access: {},
  // access 插件依赖 initial State 所以需要同时开启
  initialState: {},
};
```

### **Antd**

```ts
// config/config.ts
export default {
  antd: {
    ... // 各种配置
  },
};
```

### **布局和菜单（导航以及侧边栏）**

- **构建时配置**

```ts
// config/config.ts
export default {
  layout: {
    title: "xxx",
    locale: false, // 默认开启，如无需菜单国际化可关闭
  },
};
```

- **运行时配置**

```ts
// src/app.tsx
import { RunTimeLayoutConfig } from '@umijs/max';

export const layout: RunTimeLayoutConfig = (initialState) => {
  return {
    // 常用属性
    title: '运营后台',
    logo: 'xxx.svg',

    // 默认布局调整
    rightContentRender: () => <RightContent />,
    footerRender: () => <Footer />,
    ... //其他配置
  };
};
```

- **扩展的路由配置**


```ts
// config/route.ts
export const routes: IBestAFSRoute[] = [
  {
    path: "/xxx",
    component: "XxxPage",
    icon: "testicon",
    // 权限配置，需要与 plugin-access 插件配合使用
    access: "canRead",
    // 不展示菜单
    menuRender: false,
    // 不展示菜单顶栏
    menuHeaderRender: false,
    // 隐藏子菜单
    hideChildrenInMenu: true,
    // 隐藏自己和子菜单
    hideInMenu: true,
    // 在面包屑中隐藏
    hideInBreadcrumb: true,
  },
];
```

### **数据流【model】：状态管理**


- **创建 Model**

  ```ts
  // src/models/counterModel.ts
  import { useState, useCallback } from 'react';

  export default function Page() {
    const [counter, setCounter] = useState(0);

    const increment = useCallback(() => setCounter((c) => c + 1), []);
    const decrement = useCallback(() => setCounter((c) => c - 1), []);

    return { counter, increment, decrement };
  };
  ```

  🌸 对于 Model 文件 counterModel.ts，它的命名空间为 counterModel

  💻 Model 中允许使用其它 hooks


- **使用 Model**

  ```ts
  // src/components/Counter/index.tsx
  import { useModel } from 'umi';

  export default function Page() {
    const { counter } = useModel('counterModel');

    return (
      <div>{ counter }</div>
    );
  }
  ```

  💡 useModel() 方法传入的参数为 Model 的命名空间


- **性能优化（可选的第二个参数）**

  ```ts
  // src/components/CounterActions/index.tsx
  import { useModel } from 'umi';

  export default function Page() {
    const { add, minus } = useModel('counterModel', (model) => ({
      add: model.increment,
      minus: model.decrement,
    }));

    return (
      <div>
        <button onClick={add}>add by 1</button>
        <button onClick={minus}>minus by 1</button>
      </div>
    );
  };
  ```

  🍃 当组件只需使用 Model 中的 **部分参数** ，且对其它参数变化不感兴趣时，可传入一个函数进行 **过滤** 。


- **全局初始状态（特殊）**

  ```ts
  // src/app.ts;
  export async function getInitialState() {
    return xxx;
  }
  ```

  ```ts
  export default function Page() {

    const { initialState, 
            loading, error, 
            refresh, setInitialState 
          } = useModel('@@initialState');

    return <>{initialState}</>;
  };
  ```

    🌸 initialState：导出的 getInitialState() 方法的返回值

    🐎 loading： getInitialState() 或 refresh() 方法是否正在进行中。

    📖 error：报错的错误信息

    🍃 refresh：重新执行 getInitialState 方法，并获取新的全局初始状态

    🍂 setInitialState：手动设置 initialState 的值，手动设置完毕会将 loading 置为 false


## **🔑 常用 API**

### **useNavigate**

```ts
declare function useNavigate(): NavigateFunction;

interface NavigateFunction {
(to: To, options?: { replace?: boolean; state?: any }): void;
(delta: number): void;
}
```

- 跳转路径（页面级别的路由）

```ts
let navigate = useNavigate();
navigate("../success", { replace: true });
```

- 返回上一页

```ts
let navigate = useNavigate();
navigate(-1);
```

### **history（旧版）**

- 跳转路径
  ```ts
  import { history } from 'umi';

  // 跳转页面
  history.push('/home');
  ```

- 返回上一页
  ```ts
  const history = createBrowserHistory();
  // 返回上一页
  history.back();
  history.go(-1);
  ```

- 🔍 和 history 相关的操作，用于获取当前路由信息、执行路由跳转、监听路由变更

#### **useNavigate 与 history 区别**

- 适用场景：

  > history: 更适合在类组件和一些更复杂的场景中使用。

  > useNavigate: 更适合在函数组件中使用，符合 React Hooks 的使用模式。

- API 使用：

  > history: 提供了更底层、更细粒度的控制方法，比如 goBack, goForward 等。

  > useNavigate: 提供了更简洁的导航方式，更适合大多数简单导航场景。

- 实现方式：

  > history: 是直接操作会话历史对象。

  > useNavigate: 是通过 React Router 的 context 提供的一个更高层的抽象。


### **useLocation**

- 类型定义
```ts
declare function useLocation(): {
pathname: string;
search: string;
state: unknown;
key: Key;
};
```

- 使用
```ts
// 获取路由参数
useLocation().search

// 相对于项目配置的 base 的路径
useLocation().pathname;

// 项目如果配置 base: '/testbase'
// /testbase/page/apple
history.location.pathname
// /page/apple
useLocation().pathname
```

### **useSearchParams**

```ts
// 当前 location /comp?a=b;
const [searchParams, setSearchParams] = useSearchParams();

searchParams.get("a"); // b

searchParams.toString(); // a=b

// location 变成 /comp?a=c&d=e
setSearchParams({ a: "c", d: "e" });
```

🌲 useSearchParams 用于读取和修改当前 URL 的 查询字符串


# **Antd | ProComponent**

### **PageContainer**


- 自带 **面包屑配置** 和 **标题**
- **附带配置**：
  - **tabList**：用于展示标签页
  - **content**：页面内容
  - **footer**：底部内容
  - **extra**：右上角额外操作区域
  - **tabBarExtraContent**：标签页右侧的额外内容

```ts
<PageContainer
  header={{
    title: '页面标题',
    breadcrumb: {
      // 会根据全局路由自动生成面包屑，无特殊需求，无需操作
    },
    extra: [
      <Button key="1">次要按钮</Button>,
      <Button key="2">次要按钮</Button>,
      <Button key="3" type="primary">
        主要按钮
      </Button>,
      <Dropdown
        key="dropdown"
        trigger={['click']}
        menu={{
          ...
        }}
      >
        <Button key="4" style={{ padding: '0 8px' }}>
          <EllipsisOutlined />
        </Button>
      </Dropdown>,
    ],
  }}
  tabBarExtraContent="测试tabBarExtraContent"
  tabList={[
    {
      tab: '基本信息', key: 'base'
    },
    {
      tab: '详细信息', key: 'info'
    },
  ]}
  footer={[
    <Button key="3">重置</Button>,
    <Button key="2" type="primary">提交</Button>,
  ]}
>
  ... // 内容
</PageContainer>
```

### **ProTable**

```ts
<ProTable<TableListItem>
  columns={columns}
  request={(params,sort,filter) => {
    // const { pageSize, current } = params
  }}
  rowKey="key"
  pagination={{ }} // 翻页设置
  search={{ }} // 搜索栏 
  actionRef={ref} // 自定义触发（reload 等）
  toolBarRender={() => [
    <Button
      key="set"
      onClick={() => {}}
    >
      赋值
    </Button>,
    <Button
      key="submit"
      onClick={() => { }}
    >
      提交
    </Button>,
  ]} 
  onSubmit={(val) => {
    // 提交表单时触发
  }}
  onReset={() => {
    // 重置表单时触发
  }}
  options={false}
  dateFormatter="string"
  headerTitle="表单赋值"
/>
```

#### **ProTable: Columns 列定义**

- hideInSearch | hideInTable
- render -> 类似于 Vue 中 slot
```ts
<template #aa="{ scope }">
    <CustomTag :color="color">{{ labelMap[scope.row.aa] }} </CustomTag>
</template>
```
<div style="height: 10px" />

```ts
render: (_, record) => {
    const { text, color } = UserListTags[record?.status];
    return <Tag color={color}>{text}</Tag>;
}
```
- initialValue
- params 参数 -> 一旦变更，request就会重新请求
- request -> Search栏：Select、Cascader..

#### **ProTable: ActionRef 手动触发**

```ts
interface ActionType {
  // 刷新
  reload: (resetPageIndex?: boolean) => void;
  // 刷新并清空,页码也会重置，不包括表单
  reloadAndRest: () => void;   
  reset: () => void;   // 重置到默认值，包括表单
  clearSelected?: () => void;   // 清空选中项
  // 开始编辑
  startEditable: (rowKey: Key) => boolean;
  // 结束编辑
  cancelEditable: (rowKey: Key) => boolean; 
}

const ref = useRef<ActionType>();

<ProTable actionRef={ref} />;

ref.current.reload();
```

### **ProForm**

- 设置默认值
  > initialValues

- 表单联动 || 依赖
  > ProFormDependency -> 动态展示

- 自定义组件
  > Form.Item 包裹后使用，支持混用

  ```ts
  const ProFormText = (props) => {
    return (
      <ProForm.Item {...props}>
        <Input placeholder={props.placeholder} {...props.fieldProps} />
      </ProForm.Item>
    );
  };
  ```

#### **formRef**

```ts
const formRef = useRef<ProFormInstance>();
// 设置表单内容
formRef?.current?.setFieldsValue({
  name: "张三",
  company: "蚂蚁金服",
});

// 获取表单信息
formRef?.current?.getFieldValue("company");

<ProForm
  title="新建表单"
  formRef={formRef}
  submitter={{}} // 提交按钮相关配置
  onFinish={async (values) => {
    // 提交表单且数据验证成功后回调事件
  }} 
>
  <ProFormText name="name" label="签约客户名称" placeholder="请输入名称" />
  <ProFormText name="company" label="我方公司名称" placeholder="请输入名称" />
</ProForm>
```

#### **ProFormFields 表单项**

- 本质 Form.Item 和 组件 的结合

  > ProFormText: FormItem + Input

  ```ts
  const ProFormText = (props) => {
    return (
      <ProForm.Item {...props}>
        <Input placeholder={props.placeholder} {...props.fieldProps} />
      </ProForm.Item>
    );
  };
  ```

- 常用表单项
  - ProFormCaptcha 验证码
  - ProFormText.Password 密码框
  - ProFormSelect 下拉框
  - ProFormCascader 级联选择器
  - ...
