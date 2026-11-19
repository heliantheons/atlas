<p align="center">
  <img src="./assets/brand/hero-ice.png" width="256" alt="Atlas logo" />
</p>

<h1 align="center">Atlas</h1>

Atlas 是管理台。一个 Turborepo 把共享门户和 Hermes、Chaos、Zwei 各自的管理界面收在一起，提供一个跨系统搜索、入口聚合、风格统一的运营体验。领域相关的共享能力放在 `packages/shared`；领域无关的基础组件统一来自 `@heliannuuthus/ui`。

Atlas is the admin workspace: a Turborepo that bundles the shared portal with focused administration apps for Hermes, Chaos, and Zwei. Shared domain utilities live in `packages/shared`; domain-independent primitives come from `@heliannuuthus/ui`.

认证使用 Aegis 的标准 OIDC code + PKCE 流程。SDK endpoint 保持认证服务的 origin，SDK 从 `/api/.well-known/openid-configuration` 发现端点；Atlas 保留多 audience 资源访问配置，访问令牌继续作为不透明值传递。`getUser()` 返回标准 ID Token claims，展示名称与头像读取 `nickname`、`picture`；`iat`、`exp` 是 Unix 秒数，`aud` 是客户端 ID 字符串或字符串数组，不代表资源 audience。

本次迁移将 SDK 固定到包含标准 OIDC 实现的 Git 提交，安装时由 SDK 的 `prepare` 构建产物，依赖与锁文件可直接用于 review 和 CI。运行 `pnpm install --frozen-lockfile` 后执行 `pnpm type-check`、`pnpm lint`、`pnpm build`；部署时与 Aegis 的 OIDC 端点协调上线。SDK 正式发布后可改回对应 npm 版本，并同步更新锁文件。