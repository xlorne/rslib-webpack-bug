# Rslib Build Bug Example project

## 依赖环境

* node:v20.12.0
* pnpm:10.12.1
* @rslib/core: 0.12.4
* @rstest/core: 0.3.0

## 问题描述

通过rslib 构建的项目在配置`rslib.config.ts`为如下配置时，dynamic-components.ts在编译完成后在dynamic-components.js下不会生成export语句

执行的打包指令:
```
pnpm run build
```

```
import { defineConfig } from "@rslib/core";

export default defineConfig({
  source: {
    entry: {
      index: ['./src/**'],
    },
  },
  lib: [
    {
      bundle: false,
      dts: true,
      format: 'esm',
    },
  ],
  output: {
    target: 'web',
  },
});

```

 如果将配置文件修改为如下:
 ```
 import { defineConfig } from "@rslib/core";

export default defineConfig({
  lib: [
    {
      format: "esm",
      syntax: ["node 18"],
      dts: true,
    },
  ],
  output: {
    target: "web",
  },
});

 ```

 如下配置时可以正常创建，这时所有的代码将会写在index.js下。

 关于dynamic-components.ts build 无法创建export的问题非常奇怪，首先代码中base64.ts可以正常的创建export语句。因此应该不是配置的问题。

 在dynamic-components.ts 中使用了 __webpack_init_sharing__ 一些操作，不清楚是不是与其相关。

