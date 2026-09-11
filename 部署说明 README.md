# 人形机器人市场情报看板 · 自托管部署包

## 文件构成

| 文件 | 说明 |
|---|---|
| `index.html` | 看板主页面（单文件，含全部页面逻辑与交互） |
| `data.js` | 数据文件（全部图表与列表的数据源，已固化 217 条资讯 / 191 条事件 / 261 条指标 / 22 家企业 / 41 笔融资事件） |

> 仅需这两个文件，无任何后端依赖、无平台容器、无「豆包AI生成」角标。

## 部署方式（任选其一）

### 1. Nginx（推荐）
```nginx
server {
    listen 80;
    server_name 你的域名或IP;
    root /var/www/humanoid-dashboard;   # 指向本目录
    index index.html;
    location / { try_files $uri $uri/ /index.html; }
    location /assets/ { expires 1y; add_header Cache-Control "public, immutable"; }
    gzip on;
    gzip_types text/css application/javascript application/json;
}
```
将 `index.html` 与 `data.js` 放入 `/var/www/humanoid-dashboard/`，重启 Nginx 即可。

### 2. 静态托管平台（Vercel / Netlify / 腾讯云COS / 阿里云OSS / GitHub Pages）
直接把 `index.html` 与 `data.js` 两个文件上传即可，无构建步骤。

### 3. 本地打开
双击 `index.html` 即可在浏览器中查看（数据已固化，无需联网取数；图表库 ECharts 从 CDN 加载，首次打开需联网）。

## 数据更新方式

看板数据全部来自 `data.js`。更新数据时：
1. 在飞书数据库中更新数据（或由 AI 重新生成数据文件）；
2. 用新的 `data.js` **替换服务器上的同名文件**（其余文件不动）；
3. 建议同时更新 `index.html` 中「数据更新：YYYY-MM-DD」的日期（搜索 `D.meta.updated` 所在的数据文件即可，日期在 data.js 顶部）。

## 页面说明

- 顶部导航：市场规模 / 竞争格局 / 应用场景 / 融资情况 / 技术政策 / 资讯中心
- 市场规模：4 项 KPI + 全球/中国市场规模与出货量 4 图（支持按机构口径切换，多机构并存标注）
- 竞争格局：最新竞争动态 / 企业出货份额（赛迪/Omdia 双口径切换）/ 出货集中度 / 产品矩阵 / 价格带 / 22 家企业竞争状态 / 时间轴
- 应用场景：最新应用动态 / 场景全景 / 场景出货占比 / 落地成熟度 / 场景×任务×产品 / 时间轴
- 融资情况：6 项 KPI / 2020-2026 融资趋势 / 中国 vs 海外 / 轮次结构 / 资金流向 / 企业融资排行榜（可排序）/ 最新融资事件 41 笔 / 融资时间轴
- 技术政策：技术发展 + 政策资讯
- 资讯中心：217 条资讯，支持搜索与分类筛选
- 右上角「雾青 / 深色」主题切换

## 数据口径声明

- 多机构口径差异（如 2025 全球出货：IDC 17700 台 / 赛迪 17000 台 / Omdia 1.3 万台）已在页面分别标注来源，不做合并
- 融资金额按披露口径统计，美元按 1:7.1、欧元按 1:7.7 近似折算；「未披露」不编造
- 软银 60 亿美元控股 1X 标记「待确认(拟议)」
- 所有数字均来自公开信源，来源可追溯
