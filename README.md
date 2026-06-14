# nacos-frontend

这个仓库用于存放二开后的 Nacos 前端代码，建议直接把 `console-ui-next` 目录内容放到仓库根目录。

## 推送代码

```bash
git clone https://github.com/archinfra/nacos-frontend.git
cd nacos-frontend
# 把 console-ui-next 目录里的内容复制到这里，根目录需要有 package.json
git add .
git commit -m "feat: import nacos frontend"
git push origin main
```

## GitHub Actions 构建

`.github/workflows/frontend-ci.yml` 会在 push、PR、手动触发时执行：

- 如果存在 `pnpm-lock.yaml`：使用 pnpm
- 如果存在 `yarn.lock`：使用 yarn
- 如果存在 `package-lock.json`：使用 npm ci
- 否则使用 npm install

然后执行：

```bash
npm run build
```

或者对应包管理器的 `build` 命令。

构建产物会上传到 Actions Artifacts，保留 14 天。默认收集：

```text
dist/
build/
out/
.output/
```

## 与后端联调

前端开发阶段建议通过 Vite dev server 的 proxy 转发到后端：

```text
/v3 -> http://localhost:8080
/v1 -> http://localhost:8080
```

如果只走 GitHub Actions 构建，先确保 `src/api/registry.ts` 的管理端 API 基础路径是：

```ts
const BASE = 'v3/admin/ai/registry';
```

运行时客户端接口保持：

```ts
const CLIENT_BASE = 'v3/client/ai/registry';
```
