# monorepo packages 手順

※ 在 `yarn 1` 版本中不能使用此种方式，报错：

```shell
yarn workspace v1.22.19
error Unknown workspace "xxxxx".
```

`yarn 3 (v3.6.0)` 中尝试可行。



### 新建子项目

进入项目根目录，创建`packages`文件夹

```shell
mkdir packages
```

主项目的 `package.json` 里加入

```json
  "workspaces": [
    "packages/**"
  ],
```

进入`packages`目录下

```shell
cd packages
```

执行create命令

- 子项目名为创建的项目文件名，例如：`annotation`
- 包名需要在子项目 `package.json` 里修改 `name` 对应的名字，例如：`@svf/annotation`

```shell
yarn create vite 子项目名 --template react-ts
```



### 子项目里追加Test Components

举例2方法

#### 方法一：单组件

`src` 目录下创建 `components` 目录，创建test文件：`TestComponent.tsx`

```tsx
const TestComponent = () => {
	return <div>1111111</div>
}

export default TestComponent;
```

`src` 目录下创建 `index.ts`

```tsx
import TestComponent from "./components/TestComponent";

export default TestComponent;
```

#### 方法二：多组件

`src` 目录下创建 `components` 目录，创建test文件：`TestComponent1.tsx`，`TestComponent2.tsx`

```tsx
const TestComponent1 = () => {
	return <div>1111111</div>
}

export default TestComponent1;
```

```tsx
const TestComponent2 = () => {
	return <div>2222222</div>
}

export default TestComponent2;
```

`components` 目录下创建 `index.ts`

```tsx
export { default as TestComponent1 } from "./TestComponent1";
export { default as TestComponent2 } from "./TestComponent2";
```

`src` 目录下创建 `index.ts`

```tsx
export * from "./components";
```

#### 最后

`package.json` 里 设置项目入口

```json
"main": "src/index.ts"
```



### 主项目引入子项目

回退到项目根目录

```shell
cd ..
```

安装项目依赖

```shell
yarn install
```

配置子项目到主项目：

- 主项目名为主项目 `package.json` 里 `name` 对应的名字
- 子项目名为子项目 `package.json` 里 `name` 对应的名字

```shell
yarn workspace 主项目名 add 子项目名
```



### 主项目移除子项目包

- 子项目包名为主项目的 `package.json` 里引用的子项目的包名

```shell
yarn remove 子项目包名
```



### vite build 配置

`vite.config.ts` 中加入 `build.lib` 配置项，此为vite的库模式（library mode）

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
    lib: {
      entry: resolve(__dirname, './src/index.ts'),
      name: 'packageName',
      fileName: (format) => `packageName.${format}.js`
    },
    rollupOptions: {
      external: ["react", "react-dom"],
      output: {
        globals: {
          react: 'React',
          'react-dom': 'ReactDom',
        }
      }
    },
    outDir: 'lib/dist',
    cssCodeSplit: true
  }
})

```

修改 `tsconfig.json`

```tsx
//    "noEmit": true,
    "emitDeclarationOnly": true, /* Output only the declaration file. */
    "declaration": true, /* Generates corresponding '.d.ts' file. */
    "declarationDir": "lib/dist", /* Declaration file output directory. */
```

`package.json` 里调整build和tsc的顺序

```json
"scripts": {
  "build": "tsc && vite build",
},
```

改为

```json
"scripts": {
  "build": "vite build && tsc",
},
```

vite build 命令生成的构建会重新覆盖原有的目录，如果在tsc命令后执行，则生成的.d.ts文件会被覆盖。

tsc在构建过程中是类型检测，vite可以直接将ts转译到js

之后运行build

```shell
npm run build
```



### 手动配置 `package.json`

打包目录下添加`package.json`

`"import": "./dist/packageName.es.js"` 这行必须要加，作为暴露给使用import导入组件时候的入口起点

同样如果用require引用的话，需要加入 `"require": "./dist/packageName.umd.js"`

`"types": "./dist/index.d.ts"` 是为了exports抛出 `index.d.ts` 声明文件。`types` 也可以为 `index`, `default`等。如果不exports出`index.d.ts`，需要在主项目下新建`index.d.ts`，并 `declare model 'packageName';`

```json
{
  "name": "packageName",
  "version": "0.0.1",
  "main": "./dist/packageName.umd.js",
  "module": "./dist/packageName.es.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/packageName.es.js",
      "require": "./dist/packageName.umd.js",
      "types": "./dist/index.d.ts"
    },
    "./package.json": "./package.json"
  },
  "scripts": {
    "dev": "vite",
    "build": "vite build && tsc",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.15",
    "@types/react-dom": "^18.2.7",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "@vitejs/plugin-react": "^4.0.3",
    "eslint": "^8.45.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.3",
    "typescript": "^5.0.2",
    "vite": "^4.4.5"
  }
}
```



### `npm init` 指令

npm官网指令：https://docs.npmjs.com/cli/v6/commands/npm-init

生成 `package.json`

```shell
$ npm set init-author-name 'my name'
$ set init-author-email '12345@test.com'
$ set init-author-url 'http://test.com'
$ npm set init-license 'MIT'

$ npm init
```



### npm打包

打包目录下执行，打包.tgz包

```shell
npm pack
```



### 本地包引用方法

拷贝整个`lib/dist`文件夹到目标项目根目录下

拷贝上述`package.json`到 `lib` 目录下与`dist`同级

运行

```shell
npm i ./lib
```

