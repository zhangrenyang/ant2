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
yarn add webpack webpack-cli webpack-dev-server mini-css-extract-plugin babel-loader css-loader autoprefixer postcss-loader less-loader less @babel/core @babel/preset-react @babel/preset-env  @babel/plugin-transform-typescript  @babel/plugin-transform-runtime --dev

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
export type { ButtonProps } from './button';
export  {Button} from './button';
```

### 2.8 button\index.tsx
components\button\index.tsx
```js
export {default as Button} from './button';
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