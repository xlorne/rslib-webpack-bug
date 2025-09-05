# Rslib Build Bug Example project

## Dependency Environment
	•	node: v20.12.0
	•	pnpm: 10.12.1
	•	@rslib/core: 0.12.4
	•	@rstest/core: 0.3.0

## Problem Description
When building a project with rslib and configuring rslib.config.ts as follows, after compilation the file dynamic-components.ts will not generate export statements in the output dynamic-components.js.
**Build Command Executed:**
```
pnpm run build
```
**Config:**
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
If the configuration file is modified as follows:
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
With this configuration, it can be created normally, and all code will be written into index.js.

Regarding the issue of dynamic-components.ts not generating exports when building, it is very strange because in the code base64.ts can generate export statements normally. Therefore, it should not be a configuration problem.

In dynamic-components.ts, some operations using __webpack_init_sharing__ are included, and it is unclear whether this is related.
