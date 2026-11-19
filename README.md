# 網站工程師面試：架構學習路線圖（8 週）

> 目標：以「架構判斷」與「基礎知識」為主軸準備網站工程師面試。
> 時間：8 週 × 每週 8 小時，共 64 小時｜建立日期：2026-11-19

## 導覽

| 週 | 主題 | 本週產出 |
| --- | --- | --- |
| [總覽](#總覽) | 深度定義、時數分配、學習節奏 | — |
| [第 1 週](#第-1-週需求與前端渲染策略) | 需求與前端渲染、SPA/PWA | teatime 的 ADR |
| [第 2 週](#第-2-週bffapi-風格與後端架構) | BFF、API 風格、後端架構 | API 選型比較表 |
| [第 3 週](#第-3-週即時通訊訊息佇列關聯式資料庫) | 即時通訊、訊息佇列、關聯式資料庫 | Redmine bot 非同步架構圖 |
| [第 4 週](#第-4-週資料層帶過部署平台與服務模式) | 資料層帶過、部署平台與服務模式 | SaaS vs 私有部署比較 |
| [第 5 週](#第-5-週cicd) | CI/CD | 一條實際跑得動的 pipeline |
| [第 6 週](#第-6-週身分權限web-安全擴展性) | 身分權限、安全、擴展性 | 登入流程序列圖 + 安全檢查表 |
| [第 7 週](#第-7-週可觀測性可靠性aiagent-架構) | 可觀測性、可靠性、AI/Agent 架構 | Agent 平台架構圖 + 評估設計 |
| [第 8 週](#第-8-週整合演練) | 整合演練 | 3 題系統設計、面試故事稿 |
| [附錄](#跳過項目與參考資源) | 跳過項目與參考資源 | — |

---

## 總覽

8 週、每週 8 小時，共 64 小時：前 7 週學 35 個項目，第 8 週留給系統設計模擬和面試故事。每週最後都要產出一份實際文件，而且盡量用自己的專案當案例。

### 深度定義與時數

| 深度 | 學完要能做到 | 每項時數 | 項目數 |
| --- | --- | --- | --- |
| 深度學習 | 講得出取捨、反例與失敗情境，能在系統設計題主動選用 | 2.5–3.5 小時 | 13 |
| 知道原理 | 說得出運作方式、什麼時候用、什麼時候不用 | 1–2 小時 | 12 |
| 簡單帶過 | 能用一句話解釋它解決什麼問題 | 15–30 分鐘 | 10 |
| 跳過 | 這次不準備 | 0 | 3 |

### 每週學習節奏

先讀概念（約 40%），再用 AI agent 做一個最小實驗觀察行為（約 30%），最後寫產出、用自己的話說一遍（約 30%）。不寫 code 也沒關係，但要親眼看過它跑起來的樣子，面試才講得出細節。

---

## 第 1 週：需求與前端渲染策略

本週目標：給一個產品情境，能在 2 分鐘內說出該選哪種渲染策略、為什麼。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| 非功能需求分析 | 簡單帶過 | 0.5 | 規模、SLA、預算、團隊各自影響哪些技術決策？ |
| 框架選型 | 簡單帶過 | 0.5 | 選框架時，為什麼團隊與生態系比效能重要？ |
| 渲染策略 CSR/SSR/SSG/ISR/Islands | 深度學習 | 3.5 | 每種策略的 TTFB、FCP、互動時間差在哪？什麼是 hydration？串流 SSR 和 React Server Components 解決什麼？同一個產品怎麼混用？ |
| SPA vs MPA、PWA | 深度學習 | 3 | SPA 的路由、SEO、狀態管理代價？Service Worker 生命週期、快取策略（cache-first / network-first）？PWA 在 iOS 上的限制？ |
| 複習 | — | 0.5 | 用自己的話講一遍 |

**實作與產出**

- [ ] 請 agent 用同一個頁面各做 CSR 和 SSR 版本，用 Chrome DevTools 看 Network 與 Performance 的差異
- [ ] 檢查 teatime 的 Service Worker：它快取了什麼？離線時會發生什麼？
- [ ] 寫一份 teatime 的 ADR：為什麼選 PWA 而不是原生 App、為什麼部署在 Vercel、如果使用者變 10 萬人要改什麼

參考：web.dev 的 Rendering on the Web、patterns.dev 的 Rendering Patterns、Next.js 官方文件的 Rendering 章節。

---

## 第 2 週：BFF、API 風格與後端架構

本週目標：說得清「前端和後端之間這一層」怎麼設計，以及什麼時候該拆服務。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| BFF 模式 | 深度學習 | 2.5 | BFF 和 API Gateway 差在哪？一個客戶端一個 BFF 還是共用？BFF 怎麼處理登入狀態（token 不落到瀏覽器）？Next.js 的 Route Handler 算不算 BFF？ |
| Micro-frontend | 知道原理 | 1 | Module Federation 的運作方式？什麼組織規模才值得？代價是什麼？ |
| API 風格 REST / GraphQL / gRPC | 深度學習 | 3 | REST 的資源設計、分頁、版本管理、錯誤格式、冪等？GraphQL 的 N+1 與快取難題？gRPC 為什麼適合服務間、不適合瀏覽器？OpenAPI 契約優先怎麼做？ |
| 單體 / 模組化單體 / 微服務 | 知道原理 | 1.5 | 什麼訊號代表該拆服務？微服務帶來哪些分散式問題？模組邊界怎麼切（領域驅動的 Bounded Context）？ |

**實作與產出**

- [ ] 請 agent 寫一份 teatime 的 OpenAPI 規格，你來審：資源命名、狀態碼、分頁、錯誤格式合理嗎
- [ ] 做一張 REST / GraphQL / gRPC / WebSocket 選型比較表，每種寫一個「適合」和「不適合」的真實情境
- [ ] 用你的銀行元件案練習：如果要加一個網頁管理介面，BFF 要放在哪？

參考：Sam Newman 的 BFF 文章與《Building Microservices》、Microsoft REST API Guidelines、martinfowler.com 的 MonolithFirst。

---

## 第 3 週：即時通訊、訊息佇列、關聯式資料庫

本週目標：分得清同步、非同步、推播三種通訊的適用情境。這週和你的 WebSocket 經驗與 Redmine bot 最直接相關。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| WebSocket / SSE | 深度學習 | 3 | WebSocket 和 SSE、long polling 怎麼選？水平擴展時連線會黏在哪台機器（sticky session、Redis Pub/Sub）？斷線重連與訊息補發怎麼做？心跳與負載平衡器逾時？ |
| 訊息佇列與非同步 | 深度學習 | 3 | Queue 與 Pub/Sub 差在哪？RabbitMQ 和 Kafka 各自適合什麼？at-least-once 該怎麼配合冪等？死信佇列、重試與退避？Outbox 模式解決什麼？ |
| 關聯式資料庫、ACID、索引 | 知道原理 | 1.5 | 交易隔離層級各防什麼問題？B-tree 索引什麼時候沒用？怎麼看 EXPLAIN？N+1 問題？ |
| 複習 | — | 0.5 | — |

**實作與產出**

- [ ] 畫 Redmine bot 的非同步架構圖：工單進來 → 佇列 → agent 執行 → 結果回報；標出重複工單、agent 失敗、電腦關機時各怎麼處理
- [ ] 回顧 mps_document_api 的 WebSocket：如果要支援一百台客戶端同時連線，要改什麼
- [ ] 請 agent 在 PostgreSQL 做一個會出現慢查詢的例子，用 EXPLAIN 看加索引前後的差異

參考：《Designing Data-Intensive Applications》第 7、11 章、RabbitMQ 與 Kafka 官方文件、use-the-index-luke.com。

---

## 第 4 週：資料層帶過、部署平台與服務模式

本週目標：資料層每項能一句話說明；平台部分能依需求選出部署方式與交付模式。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| NoSQL、Redis 快取、全文搜尋 | 簡單帶過 | 1 | 各自解決什麼問題？cache-aside 是什麼？ |
| 讀寫分離、分片、CAP、資料庫遷移 | 簡單帶過 | 1 | 複寫延遲會造成什麼？CAP 的取捨一句話？migration 為什麼要向後相容？ |
| 部署平台光譜 | 知道原理 | 1.5 | PaaS、Serverless、容器服務、K8s、VM 各自的控制權、成本、維運負擔？Serverless 的冷啟動與連線數限制？ |
| Docker 與 Kubernetes | 知道原理 | 2 | 容器和 VM 差在哪？Pod、Deployment、Service、Ingress 各是什麼？什麼規模才需要 K8s？ |
| 交付模式 SaaS / 私有部署 | 知道原理 | 1.25 | 私有部署對版本管理、更新、支援成本的影響？金融業為什麼常要求地端？ |
| 多租戶設計 | 知道原理 | 1.25 | 共用表 + tenant_id、每租戶一個 schema、每租戶一個資料庫三種的隔離、成本、維運取捨？「吵鬧鄰居」問題怎麼防？ |

**實作與產出**

- [ ] 用銀行元件案寫一頁 SaaS vs 私有部署比較：如果這個元件要賣給 10 家銀行，該怎麼交付
- [ ] 請 agent 把 teatime 打包成 Docker image，在本機跑起來，看懂 Dockerfile 每一行

參考：The Twelve-Factor App、Kubernetes 官方 Concepts 章節、AWS SaaS Lens（多租戶）。

---

## 第 5 週：CI/CD

本週目標：能從空白設計一條 pipeline 與環境規劃，並說明每一步為什麼存在。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| Pipeline 設計 | 深度學習 | 3 | 步驟順序為什麼這樣排（快的先跑）？「建置一次、部署多次」怎麼做？快取、平行化、monorepo 只跑有改的部分？密鑰怎麼進 pipeline？供應鏈安全（依賴掃描、SBOM、image 簽章）？ |
| 環境規劃與預覽環境 | 深度學習 | 2.5 | Dev / Staging / Prod 怎麼切？設定與程式碼分離？PR 預覽環境的資料庫怎麼處理？Staging 為什麼常常不像 Prod？ |
| 分支策略 | 知道原理 | 1 | Trunk-based 和 Git Flow 各適合什麼交付模式？ |
| 部署策略 | 知道原理 | 1 | Rolling、藍綠、金絲雀的風險與成本？怎麼自動回滾？ |
| Feature flag、IaC | 簡單帶過 | 0.5 | 「部署」和「上線」為什麼要分開？Terraform 解決什麼？ |

**實作與產出**

- [ ] 請 agent 幫 teatime 建一條 GitHub Actions：lint → 測試 → 建置 → 部署 Vercel 預覽環境→ 合併後上線；故意讓一個測試失敗，練習看 log 找原因
- [ ] 寫一份私有部署版的 pipeline 設計（接續第 4 週）：版本號、安裝包、交付給客戶的流程和 SaaS 有什麼不同

參考：GitHub Actions 官方文件、《Continuous Delivery》（Humble & Farley）、trunkbaseddevelopment.com、SLSA 供應鏈安全框架。

---

## 第 6 週：身分權限、Web 安全、擴展性

本週目標：三個都是深度項目，也是系統設計題一定會被追問的地方，整週專心這三項。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| 身分與權限 | 深度學習 | 3 | Session 與 JWT 的撤銷、儲存位置取捨？OAuth2 授權碼流程 + PKCE 每一步在防什麼？OIDC 和 OAuth2 差在哪？refresh token 輪換？RBAC 與 ABAC？SSO 怎麼串企業的 IdP？ |
| Web 安全 | 深度學習 | 2.5 | OWASP Top 10 每項的防禦層在哪？XSS、CSRF、SSRF 的差異？CORS 實際保護了什麼、沒保護什麼？CSP、Cookie 屬性（HttpOnly、SameSite）？密鑰管理？ |
| 擴展性 | 深度學習 | 2.5 | 為什麼服務要無狀態？CDN 快取什麼、怎麼失效？限流演算法（token bucket、sliding window）與放在哪一層？一台機器到百萬使用者的演進順序？瓶頸怎麼找？ |

**實作與產出**

- [ ] 畫一張 OAuth2 + PKCE 登入流程序列圖，搭配第 2 週的 BFF：token 放哪、誰持有、怎麼更新
- [ ] 用 OWASP Top 10 審一個你的專案（銀行元件或 Redmine bot），寫成安全檢查表
- [ ] 練習一題：「將 teatime 擴展到百萬使用者」，講出每個階段會先壞哪裡

參考：oauth.net 的 OAuth 2.0 Simplified、OWASP Top 10 與 Cheat Sheet Series、《System Design Interview》（Alex Xu）第 1、4 章。

---

## 第 7 週：可觀測性、可靠性、AI/Agent 架構

本週目標：把你的 agent 團隊經驗轉成「可上線的產品架構」語言。這是你面試最有區隔度的一週。

| 項目 | 深度 | 時數 | 學完要能回答 |
| --- | --- | --- | --- |
| 可觀測性與 SLO | 知道原理 | 1 | Log、Metrics、Tracing 各回答什麼問題？SLI / SLO / error budget 怎麼訂？ |
| 可靠性 | 知道原理 | 1 | RTO / RPO？重試為什麼要配合冪等與退避？熔斷器？ |
| LLM 呼叫架構 | 知道原理 | 1 | 為什麼一定要經過後端？成本控管（快取、模型分流、用量上限）？供應商故障時怎麼降級？ |
| Agent 長任務架構 | 深度學習 | 2.5 | 長任務為什麼要用佇列 + worker？狀態怎麼存、中斷怎麼續跑？串流進度怎麼回傳前端（SSE）？工具權限、沙箱、人工審核關卡？prompt injection 怎麼防？ |
| AI 評估與取捨 | 深度學習 | 2.5 | 離線評估集怎麼建？LLM-as-judge 的限制？線上怎麼監控品質？品質、延遲、成本三者怎麼權衡？怎麼證明改了 prompt 沒有變差？ |

**實作與產出**

- [ ] 把你的 Paperclip agent 團隊重畫成「多使用者可用的 Agent 平台」架構圖：API → 佇列 → worker → 狀態儲存 → 串流回前端，加上監控與成本控管
- [ ] 用你「完成條件」與 review agent 的設計經驗，寫一份 AI 功能評估設計：指標、評估集、何時交給人確認

參考：Google SRE Book（SLO 章節）、Anthropic 的 Building Effective Agents、OWASP Top 10 for LLM Applications。

---

## 第 8 週：整合演練

本週目標：把前 7 週串成面試現場講得出來的答案。每題都照「需求 → 前端 → API → 資料 → 平台 → CI/CD → 非功能需求」的順序走。

| 活動 | 時數 | 內容 |
| --- | --- | --- |
| 系統設計模擬 | 4.5 | 3 題，每題 45 分鐘作答 + 45 分鐘自評 |
| 面試故事稿 | 2 | 3 個專案故事，每個 2 分鐘版 |
| 弱項補強 | 1.5 | 回頭複習模擬中卡住的項目 |

**系統設計題目**（每題都覆蓋到你選的深度項目）

1. 即時客服聊天室：WebSocket、佇列、擴展性、登入權限
2. 企業用 AI 文件助理 SaaS：BFF、Agent 長任務、多租戶、AI 評估、安全
3. 電商商品頁與結帳：渲染策略、CDN、API 設計、冪等、CI/CD 與部署策略

**面試故事稿**（每個都講「情境 → 我的決策 → 取捨 → 結果 → 如果重來」）

- [ ] 銀行元件升級案：法規限制下的架構與交付
- [ ] Agent 團隊：角色分工、完成條件、過度驗證問題怎麼診斷與修正
- [ ] Redmine bot 或 teatime：從需求到上線的完整決策
- [ ] 每題模擬結束後，請 AI 扮面試官追問 3 個「如果」問題（流量翻 10 倍、某服務掛掉、預算減半）

---

## 跳過項目與參考資源

**這次跳過**：法規與資安對架構的影響、計價與用量計量、RAG 架構。如果之後投金融業職缺，建議把「法規與資安」升到簡單帶過，用你的銀行經驗就能講；RAG 則是目前 AI 職缺的常見考題，投 AI 應用職缺前可以花 1 小時補上。

**貫穿整個計畫的主要資源**

| 資源 | 用在 |
| --- | --- |
| 《Designing Data-Intensive Applications》 | 第 3、4 週的資料與訊息基礎 |
| 《System Design Interview》Vol. 1、2（Alex Xu） | 第 6、8 週的系統設計練習 |
| patterns.dev、web.dev | 第 1 週的前端架構 |
| OWASP Cheat Sheet Series | 第 6 週的安全 |
| Google SRE Book | 第 7 週的可觀測性與可靠性 |
| 各大公司工程部落格 | 每週挑一篇和主題相關的真實案例 |

以上資源都是長期常用的經典教材，版本與網址請以官方最新版為準。
