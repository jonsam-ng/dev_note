# 组件库 Getting Start 总结

## xy-element (组件库)
现在的组件库主要针对 web 端。
下面是xy-element 的相关链接：

> Github: [xy-element](https://github.com/xylink-com/xy-element)
> docs: [xy-element website](https://xylink-com.github.io/xy-element/zh-CN)
> getting start: [Getting Started](https://xylink-com.github.io/xy-element/zh-CN/pages/getting-started)

### 现状

现在已经推进并完成的相关工作：

- [x] 文档管理与发布
- [x] storybook 开发预览组件
- [x] 组件打包与部署
- [x] 生成组件模板
- [x] 单元测试

#### TODO
- [ ] 组件库主题配置
- [ ] 组件 UI 和交互规范
- [ ] 基础组件开发

### 构架
- 文档：[dumi](https://d.umijs.org/zh-CN)
- 打包：rollup
- 组件管理：storybook
- 测试：jest
- 开发语言：typescript

 ### 开发指南

1. 从 github 拉取 xy-element 项目：[xy-element](https://github.com/xylink-com/xy-element)

```bash
git clone git@github.com:xylink-com/xy-element.git
```

2. 初始化项目：安装依赖

```bash
yarn init
```

3. 生成目标组件

```bash
yarn generate ComponentNameWithCamelCase
```

或者使用:

```bash
yarn gen ComponentNameWithCamelCase
```

生成组件目录如下：

```txt
Scroller
├── demo 引入文档的 demo 目录
├────── basic.tsx 默认的文档 demo 示例
├── Scroller.scss 组件样式文件
├── Scroller.stories.tsx 组件 demo 或者案例文件
├── Scroller.test.tsx 组件测试文件
├── Scroller.tsx 组件主体文件
├── Scroller.types.ts 组件类型文件
├── index.en-US.md 组件英文文档
├── index.zh-CN.md 组件中文文档
└── index.tsx 组件入口，导出相关组件和接口
```

![生成组件目录结构.png](./image/生成组件目录结构.png)

4. 启动storybook开发组件

```bash
yarn storybook
```

或者使用:

```bash
yarn story
```

打开浏览器: http://localhost:6006

![[打开storybook.png]]

5. 编写组件 demo

在 ***.stories.tsx** 中编写 demo，并且在 demo 目录中以默认导出的方式导出。
如在 demo 目录中 basic.tsx:

```TS
/**
 * title: Basic Usage // 案例英文标题
 * desc: Set interval, reset interval and clear interval via setInterval. // 案例英文描述
 *
 * title.zh-CN: 基础用法 // 案例中文标题
 * desc.zh-CN: 通过 setInterval 实现的setInterval, Reset 和 Clear 定时器的用法。 // 案例中文描述
 */
// 从 storybook 中导入组件
import { SimpleContainerWithBar } from '../ContainerWithBar.stories';
// 将组件实例默认导出
export default () => <SimpleContainerWithBar />;
```

storybook 允许一个组件有多个 demo,  只要在  story.tsx 文件中 export 多个 demo 实例就可以。

demo 案例可以在文档中引入，如：

```html
<code src="./demo/basic.tsx" />
```

或者也可以直接在文档中以 tsx/jsx 的方式引入代码块渲染为 demo。

更多文档的配置项参见：[TSX/JSX 支持的 FrontMatter 配置项](https://d.umijs.org/zh-CN/config/frontmatter#tsxjsx-%E6%94%AF%E6%8C%81%E7%9A%84-frontmatter-%E9%85%8D%E7%BD%AE%E9%A1%B9)

6. 编写文档

在 index.en-US.md 中编写英文文档，在 index.zh-CN.md 中编写中文文档。
文档头部的配置如下：

```txt
---
title: ContainerWithBar 组件名称
nav:
  title: Components // 组件在页面顶部导航栏的归属
  path: /components // 组件在页面顶部导航栏的路由
group:
  title: Scroll // 组件在左侧导航栏的分组
  path: /scroll // 组件在左侧导航栏分组的路由
legacy: /scroll/container-with-bar // 组件的路由
---
```

更多文档参考项请参考：[Markdown 支持的 FrontMatter 配置项](https://d.umijs.org/zh-CN/config/frontmatter#markdown-%E6%94%AF%E6%8C%81%E7%9A%84-frontmatter-%E9%85%8D%E7%BD%AE%E9%A1%B9)

7. 预览开发文档

```bash
yarn start
```

 打开浏览器： http://localhost:8000

8. 测试组件库

```bash
yarn test
```

或者：

```bash
yarn test:watch
```

9. 打包组件库

```bash
yarn build
```

或者：

```bash
yarn b
```

10. 打包组件文档

```bash
yarn build:doc
```

或者：

```bash
yarn bd
```

快速打包组件库和静态文档：

```bash
yarn go
```

11. 发布组件库

```bash
yarn pub
```

或者:

```bash
yarn p
```

11. 发布静态文档

```bash
yarn pub:doc
```

或者：

```bash
yarn pd
```

 ### Q&A
 
 1. 生成的模板文件的详细使用方法
 使用 `yarn gen ExampleComponent` 生成的文件如下：
 - ExampleComponent.tsx
 这里写组件的主要逻辑，会相应的生成组件的模板代码，如：
 
 ```ts
import React from "react";
import ExampleComponentProps from "./ExampleComponent.types";
import "./ExampleComponent.scss";

const ExampleComponent: React.FC<ExampleComponentProps> = (props) => {
  const {...restProps} = props;
  return (
    <div data-testid="ExampleComponent" {...restProps} className="" /> 
  )
};

ExampleComponent.displayName = "ExampleComponent";
export default ExampleComponent;
 ```
 
 可以专注于写组件的业务逻辑。
 
 - ExampleComponent.scss
 
 这里写组件样式，主题文件会自动引入。
 
 ```ts
 @import "src/theme.scss";
 ```
 
 - ExampleComponent.types.ts 
 
 这里写组件的类型、接口等。
 
 ```ts
export default interface ExampleComponentProps {
}
```

- ExampleComponent.stories.tsx

这里是组件 demo ，可以打开 storybook 看到实时的开发效果。这里会导出一个默认的 demo 。

```ts
import React from "react";
import ExampleComponent from "./ExampleComponent";

export default {
    title: "ExampleComponent" // storybook 侧边导航栏标题
};

export const BasicExampleComponent = () => <ExampleComponent />;
```

- ExampleComponent.test.tsx
这里是测试文件，可以使用 jest 和 testing-library 进行单元测试。

```ts
import React from "react";
import { render } from "@testing-library/react";

import ExampleComponent from "./ExampleComponent";
import ExampleComponentProps from "./ExampleComponent.types";
```

- index.zh-CN.md 和 index.en-US.md

这里是中英文文档，生成的文件中已经有默认的配置，写文档时只要稍作修改即可。

```md
---
title: ExampleComponent
nav:
  title: Components
  path: /components
group:
  title: Example
  path: /example
legacy: /example/component-name
---

# ExampleComponent
// 组件简介

## 示例

<code src="./demo/basic.tsx" />

## 用法

\`\`\`ts

\`\`\`

### 参数

| 参数 | 描述 | 类型 | 默认值 |
| -------- | ----------- | ---- | ------- |

### 接口与类型

\`\`\`ts

\`\`\`

### 工作流

#### TOFIX

#### TODO

```

默认文档中包含了实例、用法、参数、接口与类型、TODO 等项，可以介绍清除组件的使用方法、案例和开发计划。

- index.tsx

这个文件会帮你导出组件、组件类型，总结组件需要向外暴露的部分。

```ts
import ExampleComponent from "./ExampleComponent";
import ExampleComponentProps from "./ExampleComponent.types";

export { ExampleComponentProps };
export default ExampleComponent;
```

 2. 组件和文档分别是怎样部署的？
 
 - 组件由 master 分支维护，分版本发布到 npm。发布方式为手动发布。
 - 文档的更新与发布：
   - 目前文档的发布方式：文档原件保存在 guide 目录，打包后在 docs 目录。打包文档后更新至 master 分支，发布到 github-pages。
   - 计划的发布方式：开发人员不再关注文档打包和部署，开发时 git 不追踪 docs 目录，开发者将分支合并至 master 后，由 GitHub action 打包项目文档，并发布到 gh-pages分支作为静态文档发布。发布方式为自动发布，这样开发者只需关注文档编写即可。
 
 3. 为什么要同时使用 dumi 和 storybook？
 
 - storybook 开发组件很方便，但是生成静态文档不是很方便。
 - dumi 只作为文档工具来使用，即我们写的文档最后会被自动打包和发布，开发中最常使用的仍然是 storybook。
 - 后期我们可以将 storybook 的组件文档也进行发布，用户既可以在 storybook 中看到浏览各种组件，也可以在 dumi 中看到组件的使用方法和开发动向。
 - 开发中只启用 storybook 即可，编写文档时可以本地打开 dumi 查看文档效果。
  
 4. 怎么更改组件模板？
  组件模板可以在 utils/templates 目录中进行修改，可以新增模板文件或者修改现有的模板。
  
  5. storybook 中开发的 demo 能用于文档中吗？
  可以。在 demo 目录中象征性的从 story 文件中导入再默认导出 demo，然在在文档中通过 code 标签直接引用，demo 无需重复编写。
  或者您也可以直接在文档中以 tsx/jsx 代码段从 story 文件中中引入 demo 并显示在文档中。
  
  6. 开发人员需要做什么？
  只需要关注组件逻辑和文档，不需要关注部署与发布。
  
  > 1. yarn gen Component
  > 2. yarn story
  > 3. 编写组件和 demo
  > 4. 填写文档
  > 5. pull request
 
 
## toolkit (工具库)

> Github: [toolkit](https://github.com/xylink-com/toolkit)
> docs: [xy-element website](xylink-com.github.io/toolkit/zh-cn)
> getting start: [Getting Started](https://xylink-com.github.io/toolkit/zh-CN/pages/document)

### 计划开发的工具类

工具类正在开发，没人每周开发并提交一个工具类，由团队内进行代码审核。

 - [x] broswer
 - [x] fullscreen(event)
 - store
 - [x] time
 - axios
 - httpclient
 - 节流/防抖
 - websocket
 - logger
 - timeout/interval/requestanimation
 - emmit
 - db
 - getTime(format);
 - tranlateMoney(fixed);
 - [x] url parms
 - [x] mergeDeep
 - cloneDeep
 - array/object/mat
 - native(native-js)(android,ios)


## xy-hooks (hooks 库)

> Github: [toolkit](https://github.com/xylink-com/xy-hooks)
> docs: [xy-element website](xylink-com.github.io/xy-hooks/zh-cn)
> getting start: [Getting Started](https://xylink-com.github.io/xy-hooks/zh-CN/pages/getting-started)




