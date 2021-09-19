## 1.创建项目
### 1.创建文件夹
```js
mkdir ant
cd ant
npm init -y
```

### 2.package.json
```json
{
  "name": "@zhangry/ant",
  "version": "1.0.0",
  "description": "React组件的企业级UI设计",
  "main": "lib/index.js",
  "scripts": {
    "build": "webpack"
  },
  "publishConfig": {
    "access": "public",
    "registry": "http://registry.npmjs.org"
  },
  "homepage": "https://zhangrenyang.github.io/ant",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/zhangrenyang/ant.git"
  },
  "keywords": [
    "ant",
    "component",
    "components",
    "design",
    "framework",
    "frontend",
    "react",
    "react-component",
    "ui"
  ],
  "author": "zhangrenyang",
  "license": "MIT"
}
```

## 2.配置webpack
### 2.1 安装依赖
```js
yarn add webpack webpack-cli webpack-dev-server mini-css-extract-plugin babel-loader css-loader autoprefixer postcss-loader less-loader less @babel/core @babel/preset-react @babel/preset-env  @babel/runtime @babel/plugin-transform-typescript  typescript @babel/plugin-transform-runtime @types/node --dev

yarn add react react-dom 
yarn add @types/react @types/react-dom --dev
```

### 2.2 webpack.config.js
```js
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

const cwd = process.cwd();
module.exports = {
  mode: 'development',
  devtool: false,
  entry: {
    ant: './index.js',
  },
  output: {
    path: path.resolve('dist'),
    filename: '[name].js',
    library: 'ant',
    libraryTarget: 'umd',
  },
  externals: {
    react: {
      root: 'React',
      commonjs2: 'react',
      commonjs: 'react',
      amd: 'react',
    },
    'react-dom': {
      root: 'ReactDOM',
      commonjs2: 'react-dom',
      commonjs: 'react-dom',
      amd: 'react-dom',
    },
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js', '.jsx', '.json'],
    alias: {
      antdesign: cwd,
    },
  },
  module: {
    rules: [
      {
        test: /\.(j|t)sx?$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
      },
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: 'css-loader',
            options: {
              sourceMap: true,
            },
          },
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: ['autoprefixer'],
              },
              sourceMap: true,
            },
          },
        ],
      },
      {
        test: /\.less$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: 'css-loader',
            options: {
              sourceMap: true,
            },
          },
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: ['autoprefixer'],
              },
              sourceMap: true,
            },
          },
          {
            loader: 'less-loader',
            options: {
              lessOptions: {
                javascriptEnabled: true,
              },
              sourceMap: true,
            },
          },
        ],
      },
      {
        test: /\.(png|jpg|jpeg|gif|svg)(\?v=\d+\.\d+\.\d+)?$/i,
        type: 'asset',
      },
    ],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: '[name].css',
    }),
  ],
};
```


### 2.3 babel.config.js
babel.config.js
```js
module.exports = {
  presets: [
    '@babel/preset-react',
    [
      '@babel/preset-env',
      {
        modules: 'auto',
        targets: {
          browsers: ['last 2 versions', 'Firefox ESR', '> 1%', 'ie >= 11'],
        },
      },
    ],
  ],
  plugins: [
    [
      '@babel/plugin-transform-typescript',
      {
        isTSX: true,
      },
    ],
    ['@babel/plugin-transform-runtime'],
  ],
};

```

### 2.4 .gitignore
.gitignore
```js
*.iml
.idea/
.ipr
.iws
*~
~*
*.diff
*.patch
*.bak
.DS_Store
Thumbs.db
.project
.*proj
.svn/
*.swp
*.swo
*.log
*.log.*
*.json.gzip
node_modules/
.buildpath
.settings
npm-debug.log
nohup.out
_site
_data
dist
report.html
/lib
/es
elasticsearch-*
config/base.yaml
/.vscode/
/coverage
yarn.lock
package-lock.json
components/**/*.js
components/**/*.jsx
!components/**/__tests__/**/*.js
!components/**/__tests__/**/*.js.snap
/.history
*.tmp

# Docs templates
site/theme/template/Color/ColorPicker.jsx
site/theme/template/IconDisplay/*.js
site/theme/template/IconDisplay/*.jsx
site/theme/template/IconDisplay/fields.js
site/theme/template/Home/**/*.jsx
site/theme/template/utils.jsx
site/theme/template/Layout/Footer.jsx
site/theme/template/Layout/Header/**/*.jsx
site/theme/template/Layout/SiteContext.jsx
site/theme/template/Content/Article.jsx
site/theme/template/Content/EditButton.jsx
site/theme/template/Resources/*.jsx
site/theme/template/Resources/**/*.jsx
site/theme/template/NotFound.jsx
scripts/previewEditor/index.html
components/version/version.tsx

# Image snapshot diff
__diff_output__/
__image_snapshots__/
/jest-stare
/imageSnapshots
/imageDiffSnapshots
storybook-static

sh.exe.stackdump
/snapshots
/diffSnapshots
```

### 2.5 tsconfig.json
```json
{
    "compilerOptions": {
      "strictNullChecks": true,
      "module": "esnext",
      "moduleResolution": "node",
      "esModuleInterop": true,
      "experimentalDecorators": true,
      "jsx": "react",
      "noUnusedParameters": true,
      "noUnusedLocals": true,
      "noImplicitAny": true,
      "target": "es6",
      "lib": ["dom", "es2017"],
      "skipLibCheck": true,
      "types": ["node"]
    },
    "exclude": ["node_modules", "lib", "es"]
}
```

### 2.6 index.js
```js
module.exports = require('./components');
```

### 2.7 components\index.tsx
components\index.tsx
```js
import Button from './button';

export type { ButtonProps } from './button';
export default Button;

```

### 2.8 button\index.tsx
components\button\index.tsx
```js
import Button from './button';
export default Button;
export type { ButtonProps } from './button';
```

### 2.9 button.tsx
components\button\button.tsx
```js
import React, { ButtonHTMLAttributes } from 'react';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {}
const Button: React.FC<ButtonProps> = (props) => {
  const { children } = props;
  return <button type="button">{children}</button>;
};

export default Button;
export type { ButtonProps };
```

## 3.storybook
### 3.1 安装
```js
yarn add @storybook/react   @storybook/addon-essentials --dev
```

### 3.2 .storybook\main.js
.storybook\main.js
```js
module.exports = {
    stories: [
        "../components/Introduction.stories.mdx",
        "../components/Install.stories.mdx",
        "../components/Components.stories.mdx",
        "../components/**/*.stories.mdx",
        "../components/**/*.stories.@(js|jsx|ts|tsx)"
    ],
    addons: ['@storybook/addon-essentials'],
};
```

### 3.3 Introduction.stories.mdx
components\Introduction.stories.mdx
```js
<Meta title="开始/介绍" />

## Ant Design of React
antd 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。
```

### 3.4 Install.stories.mdx
components\Install.stories.mdx
```js
<Meta title="开始/安装使用" />

## 安装
使用 npm 或 yarn 安装


npm install ant --save



yarn add ant


## 浏览器引入
在浏览器中使用 script 和 link 标签直接引入文件，并使用全局变量 ant
我们在 npm 发布包内的 antdesign/dist 目录下提供了 ant.js

## 示例


import { Button } from 'antdesign';
ReactDOM.render(<Button>按钮</Button>, mountNode);
```

### 3.5 Components.stories.mdx
components\Components.stories.mdx
```js
<Meta title="开始/组件总览" />

## 组件总览
antd 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

## 通用
- Button 按钮
- Icon 图标
- Typography 排版

## 布局
- Divider 分割线
- Grid 栅格
- Layout 布局
- Space 间距

## 导航
- Affix 固钉
- Breadcrumb 面包屑
- Dropdown 下拉菜单
- Menu 导航菜单
- Pagination 分页
- PageHeader 页头
- Steps 步骤条

## 数据录入
- AutoComplete 自动完成
- Checkbox 多选框
- Cascader 级联选择
- DatePicker 日期选择框
- Form 表单
- InputNumber 数字输入框
- Input 输入框
- Mentions 提及
- Rate 评分
- Radio 单选框
- Switch 开关
- Slider 滑动输入条
- Select 选择器
- TreeSelect 树选择
- Transfer 穿梭框
- TimePicker 时间选择框
- Upload 上传

## 数据展示
- Avatar 头像
- Badge 徽标数
- Comment 评论
- Collapse 折叠面板
- Carousel 走马灯
- Card 卡片
- Calendar 日历
- Descriptions 描述列表
- Empty 空状态
- Image 图片
- List 列表
- Popover 气泡卡片
- Statistic 统计数值
- Tree 树形控件
- Tooltip 文字提示
- Timeline 时间轴
- Tag 标签
- Tabs 标签页
- Table 表格


## 反馈
- Alert 警告提示
- Drawer 抽屉
- Modal 对话框
- Message 全局提示
- Notification 通知提醒框
- Progress 进度条
- Popconfirm 气泡确认框
- Result 结果
- Spin 加载中
- Skeleton 骨架屏

## 其他
- Anchor 锚点
- BackTop 回到顶部
- ConfigProvider 全局化配置
```

### 3.6  button.stories.tsx
components\button\button.stories.tsx
```js
import React from "react";
import { ComponentStory, ComponentMeta } from "@storybook/react";

import Button from ".";

export default {
  title: "通用/Button(按钮)",
  component: Button,
} as ComponentMeta<typeof Button>;

const Template: ComponentStory<typeof Button> = (args) => (
  <Button  {...args} />
);

export const Basic = Template.bind({});
Basic.args = {
  children: "按钮",
};
```

### 3.7 package.json
```diff
  "scripts": {
    "build": "webpack",
+   "storybook": "start-storybook -p 6006",
+   "build-storybook": "build-storybook"
  },
```


## 4.测试
- [configuration](https://jestjs.io/docs/configuration)
- [code-transformation](https://jestjs.io/docs/code-transformation)

### 4.1 安装
```js
yarn add jest @types/jest  @wojtekmaj/enzyme-adapter-react-17 puppeteer @types/puppeteer jest-environment-puppeteer  @types/jest-environment-puppeteer jest-puppeteer  jest-image-snapshot @types/jest-image-snapshot --dev
yarn add enzyme  @types/enzyme  --dev
```

### 4.2 tests\setup.js
tests\setup.js
```js
const React = require('react');
const Enzyme = require('enzyme');

const Adapter = require('@wojtekmaj/enzyme-adapter-react-17')
Enzyme.configure({ adapter: new Adapter() });
```

### 4.3 tests\index.html
tests\index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Amazing Antd</title>
    <style>
      body {
        border: 5px solid #1890ff;
      }
    </style>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

### 4.4 unit.jest.js
unit.jest.js
```js
module.exports = {
  verbose: true,
  testEnvironment: 'jsdom',
  setupFiles: ['./tests/setup.js'],
  testMatch: ['**/unit/**/*.(spec|test).(js|ts|jsx|tsx)'],
  collectCoverage: true,
  collectCoverageFrom: [
    'components/**/*.(js|ts|jsx|tsx)',
    '!components/**/*.stories.(js|ts|jsx|tsx)',
    '!components/**/*.(spec|test).(js|ts|jsx|tsx)',
  ],
};
```

### 4.5 e2e.jest.js
e2e.jest.js
```js
module.exports = {
  verbose: true,
  testEnvironment: 'jest-environment-puppeteer',
  setupFiles: ['./tests/setup.js'],
  preset: 'jest-puppeteer',
  testMatch: ['**/e2e/**/*.(spec|test).(j|t)sx'],
};
```

### 4.6 unit\index.test.tsx
components\button\unit\index.test.tsx
```js
import React from 'react';
import { mount } from 'enzyme';
import Button from '..';

describe('Button', () => {
  it('mount correctly', () => {
    expect(() => mount(<Button>Follow</Button>)).not.toThrow();
  });
});

```

### 4.7 snapshot.spec.tsx
components\button\e2e\snapshot.spec.tsx
```js
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import { configureToMatchImageSnapshot } from 'jest-image-snapshot';
import Button from '..';
import 'jest-environment-puppeteer';
const toMatchSnapshot = configureToMatchImageSnapshot({
  customSnapshotsDir: `${process.cwd()}/snapshots`,
  customDiffDir: `${process.cwd()}/diffSnapshots`,
});
expect.extend({ toMatchSnapshot });
describe('Button snapshot', () => {
  it('screenshot should correct', async () => {
    await jestPuppeteer.resetPage();
    await page.goto(`file://${process.cwd()}/tests/index.html`);
    const html = ReactDOMServer.renderToString(<Button>按钮</Button>);
    await page.evaluate((innerHTML:string) => {
      document.querySelector('#root')!.innerHTML = innerHTML;
    }, html);
    const screenshot = await page.screenshot();
    expect(screenshot).toMatchSnapshot();
  });
});

```

### 4.8 jest-puppeteer.config.js
jest-puppeteer.config.js
```js
module.exports = {
  launch: {
    dumpio: true,
    headless: process.env.HEADLESS !== 'false',
  },
  browserContext: 'default',
};
```

### 4.9 package.json
package.json
```diff
{
  "scripts": {
    "build": "webpack",
    "storybook": "start-storybook -p 6006",
    "build-storybook": "build-storybook",
+   "test:unit": "jest --config unit.jest.js",
+   "test:e2e": "jest --config e2e.jest.js",
+   "test": "npm run test:unit && npm run test:e2e"
  },
}
```

## 5.eslint
### 5.1 安装
```js
yarn add @typescript-eslint/parser eslint eslint-plugin-import eslint-plugin-react eslint-plugin-react-hooks and eslint-plugin-jsx-a11y eslint-config-airbnb --dev
```

### 5.2 .eslintrc.js
.eslintrc.js

```js
module.exports = {
  parser: '@typescript-eslint/parser',
  extends: ['airbnb'],
  env: {
    browser: true,
    node: true,
    jasmine: true,
    jest: true,
    es6: true,
  },
  rules: {
    'import/extensions': 0,
    'import/no-unresolved': 0,
    'react/jsx-filename-extension': 0,
    // https://github.com/typescript-eslint/typescript-eslint/issues/2540#issuecomment-692866111
    'no-use-before-define': 0,
    'import/prefer-default-export': 0,
    'import/no-named-default': 0,
    'no-console': 0,
    'no-param-reassign': 0,
    'func-names': 0,
  }
};
```

### 5.3 .eslintignore
.eslintignore
```js
components/**/e2e/*
components/**/unit/*
components/**/*.stories.*
lib
es
umd
dist
.storybook
gulpfile.js
```

### 5.4 package.json
package.json
```diff
{
  "scripts": {
    "build": "webpack",
    "storybook": "start-storybook -p 6006",
    "build-storybook": "build-storybook",
    "test:unit": "jest --config unit.jest.js",
    "test:e2e": "jest --config e2e.jest.js",
    "test": "npm run test:unit && npm run test:e2e",
+   "lint": "eslint --ext .js,.jsx,.ts,.tsx components",
+   "lint:fix": "eslint --fix --ext .js,.jsx,.ts,.tsx components"
  }
}
```


## 6.prettier
### 6.1 安装依赖

```js
yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev
```

### 6.2 .eslintrc.js
.eslintrc.js
```diff
module.exports = {
  parser: '@typescript-eslint/parser',
+ extends: ['airbnb','prettier'],
  env: {
    browser: true,
    node: true,
    jasmine: true,
    jest: true,
    es6: true,
  },
+ plugins: ['prettier'],
  rules: {
+   'prettier/prettier': ['error', { endOfLine: 'auto' }],
    'import/extensions': 0,
    'import/no-unresolved': 0,
    'react/jsx-filename-extension': 0,
    // https://github.com/typescript-eslint/typescript-eslint/issues/2540#issuecomment-692866111
    'no-use-before-define': 0,
    'import/prefer-default-export': 0,
    'import/no-named-default': 0,
    'no-console': 0,
    'no-param-reassign': 0,
    'func-names': 0,
  }
};

```

### 6.3 .prettierrc
.prettierrc
```js
{
    "singleQuote": true
}  
```

### 6.4 button\index.tsx
components\button\index.tsx
```diff
+            const title = "hello";
```

### 6.5 settings.json
.vscode\settings.json
```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "afterDelay"
}
```


## 7.editorconfig
### 7.1 .editorconfig
```js
# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*.{js,css}]
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
```

## 8. git hook
### 8.1 安装
```js
yarn add husky --dev
npm set-script prepare "husky install"
npm run prepare
```

### 8.2 pre-commit
### 8.2.1 安装脚本
```js
npx husky add .husky/pre-commit "npx lint-staged"
```

### 8.2.2 .lintstagedrc
```js
{
    "*.{js,ts,jsx,tsx}": "eslint"
}
```


### 8.3 commit-msg
### 8.3.1 安装依赖
```js
yarn add commitizen cz-customizable @commitlint/cli @commitlint/config-conventional --dev
```

### 8.3.2 安装脚本
```js
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```

### 8.3.3 .cz-config.js
```js
module.exports = {
  types: [
    { value: "feat", name: "feat:一个新特性" },
    { value: "fix", name: "fix:修复BUG" },
  ],
  scopes: [{ name: "admin" }, { name: "user" }],
};
```

### 8.3.4 commitlint.config.js
commitlint.config.js
```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```


### 8.4 pre-push
### 8.3.1 安装脚本
```js
npx husky add .husky/pre-push "npm run test"

npm pubish
```

## 9. 编译
### 9.1 安装依赖
```js
yarn add rimraf gulp gulp-typescript gulp-babel merge2 --dev
```


