# 护理质控管理系统 · 管理端高保真原型 Demo

大连大学附属中山医院「护理质控管理系统」管理端的可点击高保真原型，纯静态、离线可用，通过 GitHub Actions 部署到 GitHub Pages。

## 访问方式

1. 打开 Pages 地址（Settings → Pages 中查看，形如 `https://dlldsys.github.io/nursing-qc-admin-demo/`）。
2. 输入访问密码后进入原型；密码错误时不会加载任何 demo 内容。

## 密码保护是怎么做的

Pages 是纯静态托管，没有服务端鉴权，所以这里用**内容加密**实现门禁：

- `demo.enc.json`：原型页面经 **PBKDF2-SHA256（200,000 次迭代）派生密钥 + AES-256-GCM** 加密后的密文。
- `index.html`：入口页，输入密码后在本机派生密钥并解密（GCM 认证标签校验，密码不对就解不开），解密成功才把原型写入页面。

因此仓库内容公开也不影响保密性：没有密码拿到的只是密文。**修改密码**只需重新构建（见下）。

## 目录结构

```
.
├─ index.html                 入口密码页
├─ demo.enc.json              原型密文
├─ lib/                       前端依赖（React / Ant Design / ECharts / moment / Babel）
├─ .github/workflows/pages.yml GitHub Actions 部署工作流
└─ .nojekyll
```

## 更新内容 / 改密码

在原型源码仓库执行：

```bash
node tools/build-nursing-demo-pages.mjs --password <新密码>
```

然后提交并推送到 `main`，Actions 会自动重新发布。

## 说明

- 原型内的数据均为模拟数据，仅用于演示。
- Pages 地址为公开地址，但内容受密码保护；如需彻底下线，删除仓库或在 Pages 设置中关闭发布即可。
