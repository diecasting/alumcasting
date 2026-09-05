# S.6-C.4-H — GitHub Pages 部署链路 + 线上实站只读验证

> 阶段性质：STRICTLY READ-ONLY。本阶段未修改任何源码 / 配置 / 生产设置，未 commit / push / rerun。
> 验证目标 commit：`52da2f2`（"consolidate SSM owner and repair T6 links"）
> Actions run：`33950866811`  Workflow：`Deploy Hugo to GitHub Pages`

---

## 1. Deployment identity

| Field | Value |
|-------|-------|
| Commit | `52da2f2c8767c2657c4248af4ac09b27503f32ab` |
| Run ID | `33950866811` |
| Workflow | `Deploy Hugo to GitHub Pages` |
| Event | `push` |
| Branch | `main` |
| Conclusion | `success` (build 9s + deploy 8s, both green) |
| Head SHA (verified) | `52da2f2c8767c2657c4248af4ac09b27503f32ab` — **matches commit** |
| `--log-failed` | empty (no failed-job logs) |

观察（非阻断）：run 注记 `Node.js 20 is deprecated`（actions 在 Node 24 runner 上被强制运行）。仅为弃用告警，conclusion 仍为 `success`，不影响部署结果。

**PASS 条件**：headSha = `52da2f2`、status = `completed`、conclusion = `success` —— 全部满足。

---

## 2. GitHub Pages configuration

来源：`gh api repos/diecasting/alumcasting/pages` + `gh repo view`

| Field | Value |
|-------|-------|
| Repository | `diecasting/alumcasting` |
| Source branch | `main` |
| Source path | `/` |
| Build type | `workflow`（Actions 构建，非 legacy 分支部署） |
| Custom domain | none（`cname: null`） |
| HTTPS | enforced（`https_enforced: true`） |
| Public | `true` |
| Live URL | `https://diecasting.github.io/alumcasting/` |

**PASS 条件**：source/build 与仓库实际部署方式一致，branch = `main`，非旧 repository / 旧 branch / 旧 source —— 满足。

---

## 3. Live URL verification

方法：Python `urllib` + 自定义 `NoRedirect` handler 抓取首跳状态/Location；对 3xx 再跟随至终态。UA 模拟浏览器。`github.io` 不受 WAF 限制，可直连。

> 注：`index,follow` 在生成 HTML 中写作 `content="index, follow"`（逗号后含空格），解析值为 `index, follow` = `index,follow`，正常。

| URL | HTTP | Canonical | H1 | Robots | JSON-LD | Result |
|-----|-----:|-----------|---:|--------|---------|--------|
| `/semi-solid-die-casting-heat-treatable-aluminum/` (SSM owner) | 200 | self (`…/semi-solid-die-casting-heat-treatable-aluminum/`) | 1 | index,follow | 2/2 valid (Organization, WebPage) | **PASS** |
| `/semi-solid-die-casting/` (generic SSM) | 404 | n/a | n/a | n/a | n/a | **EXPECTED ABSENCE — PASS** |
| `/t6-heat-treatment-semi-solid-die-casting-aluminum/` (dead T6) | 404 | n/a | n/a | n/a | n/a | **PASS (gone)** |
| `/cold-chamber-die-casting-services/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/custom-aluminum-die-casting-for-ev-powertrain-components/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/gravity-die-casting-manufacturer/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/liquid-cooled-aluminum-cooling-plates-for-electric-vehicles/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/manufacturing-capabilities/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/medical-device-component-machining/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |
| `/sand-casting-services/` | 200 | self | 1 | index,follow | 2/2 valid | **PASS** |

补充（Sitewide）：
- `sitemap.xml` → 200，含 **44** 个 `<loc>` URL。`owner` 在 sitemap 中 = **True**；`generic SSM` 在 sitemap 中 = **False**；`dead T6` 在 sitemap 中 = **False**；全文 `old T6` = 0，`generic SSM` = 0。
- `robots.txt` → 200；`old T6` = 0，`generic SSM` = 0。

---

## 4. Link integrity

| Metric | Value |
|--------|-------|
| Old T6 references on the 7 repaired pages | **0** |
| New SSM owner references on the 7 repaired pages | cold=1, custom=1, gravity=1, liquid=1, mfg=1, medical=2, sand=2（每页 ≥1） |
| Broken links introduced by this change | **0** |
| Sitewide old T6 URL leak (sitemap + robots + all 7 pages) | **0** |
| Sitewide generic SSM URL leak | **0**（404，未进入 sitemap/robots/正文） |

---

## 5. Regression

| Check | Result |
|-------|--------|
| SSM owner regression | PASS — 线上 200 / H1=1 / self-canonical / index,follow / JSON-LD 2/2 valid |
| Dead-link regression | PASS — 7 页 `old T6 = 0`，目标 owner HTTP 200 |
| H1 | 所有受检页 = 1（无重复 H1） |
| Canonical | 所有受检页均自引用（self-canonical） |
| Robots | 受检页均 `index,follow`（无意外 noindex） |
| JSON-LD | 所有受检页均 2/2 有效，无 malformed / HTML escape 破坏 / 脚本截断 |

本地构建回归（TASK 11）：`hugo --gc --minify` **exit 0**，无 error/warning；48 页。生成 `public/` 中 owner 存在、generic 缺、old T6 文件 = 0。

---

## 6. Production changes

| System | Changed? |
|--------|----------|
| WordPress | NO |
| Cloudflare | NO |
| DNS | NO |
| Redirects (WP Redirection) | NO |
| GitHub Pages settings | NO |
| Workflow rerun | NO |

Git 工作区（TASK 12）：`git status --short` = **empty**（clean）；`HEAD = 52da2f2c8767c2657c4248af4ac09b27503f32ab`；`content/` 无任何修改；`public/` 为 gitignored 生成物，未纳入提交。

---

## Final Decision

```text
PHASE_6_C.4-H = PASS

Commit: 52da2f2c8767c2657c4248af4ac09b27503f32ab
Actions Run: 33950866811 (conclusion=success)
Pages Source: main:/ (build_type=workflow)
Live Domain: https://diecasting.github.io/alumcasting/
Live Core URLs:
  SSM owner = 200 / self-canonical / index,follow / JSON-LD valid
  generic SSM = 404 (EXPECTED ABSENCE — PASS)
  dead T6 = 404 (gone)
Old T6 References: 0 (7 pages + sitemap + robots)
Broken Links: 0
Git Status: clean (HEAD = 52da2f2, no source changes)
Report: reports/S.6-C.4-H-github-pages-live-verification.md
```

所有关键检查通过：Actions 成功、Pages 配置与仓库一致、线上核心 URL 与 7 个修复页全部 200/自规范/JSON-LD 有效、generic SSM 与 dead T6 均为 404（无残留可索引页）、全站无旧 T6 泄漏、无引入断链、无生产侧改动。

---

## HARD STOP

完成报告即停止。未自动修复、未 commit / push、未 rerun Actions、未修改 Pages / DNS / Cloudflare / WordPress / redirect。等待下一步授权。
