# AIエージェント/AX プロダクト立ち上げ計画

> 作成: 2026-07-09 / 最終検証: 2026-11-05 / owner: hiroshi_takizawa / GitHub org: hiroshi57 (Public)
> ローカル配置: `C:/Users/hiroshi_takizawa/<repo>` / 他ツールの単純コピーは不可（差別化コア必須）
> 収益面の反証は `revenue-refutation.md`、企画群は `suite-30-plan.md` ほかを参照

---

## 0. 用語定義（この台帳での「完了」の意味）

旧版の `✅本番` を実態に合わせて 5 段階に分解した。**外部提示時はこの定義を必ず併記する。**

| 成熟度 | 定義 | 判定に必要な証跡 |
|--------|------|------------------|
| L0 repo | リポジトリが存在し push 済み | GitHub API で存在確認 |
| L1 実装 | 差別化コアがコードとして存在 | 該当モジュールのパス |
| L2 検証 | 自動テストが存在し CI が緑 | gh run list の conclusion=success |
| L3 デモ公開 | 静的/疑似データで外部から閲覧可能 | HTTP 200 + 実測バイト数 |
| L4 本番稼働 | 実データ・実ユーザー・実LLM課金で稼働し SLO/監視あり | 稼働ログ・監視・利用者数 |

**現状: L3 が 12 本、L2 未達が 2 本（CI 失敗1・CI 無し1）、公開停止が 1 本。L4（本番稼働）は 0 件。**
旧記載の `✅本番` は「デモ公開」の意味であり、実ユーザー稼働ではない。営業資料での「本番稼働」表記は不可。

---

## 1. 開発順

6 → 5 → 1 → 4 → 2 → 3 → 7 → 8 → B/C系（順序未確定） → A1 corral（2026-07-24 以降、実質最優先）

---

## 2. タスク台帳

> test 列は `def test_` の実測数（2026-11-05）。旧版の test28 等は 2026-07 時点の値で、
> 現在は増えている（例: #1 は 28 → 102、#6 は 31 → 226）。数値を引用する際は測定日を併記する。

### 原案 1-8

| # | repo | 概要 | 差別化コア | test | CI | 成熟度 |
|---|------|------|-----------|------|----|-------|
| 1 | dify-rag-approval-agent | Dify社内RAG+承認フロー | 引用必須+監査ログ／規程改定差分追跡+回答陳腐化検知 | 102 | ❌ **最新 run が failure (2026-08-20)** | L2未達 |
| 2 | tool-calling-agent-mvp | function callingツール操作 | ドライラン+可逆ログ／操作リスクスコア+承認ポリシー | 25 | ❌ **ワークフロー無し** | L2未達 |
| 3 | agent-roi-poc | ROI導入前後比較PoC | 信頼区間+人間介入率／因果推論ROI(合成対照法+DID+プラセボ検定) | 27 | ✅ (2026-10-20) | L3 |
| 4 | context-rerank-search | RAG+リランキング検索 | 根拠説明+eval同梱／反実仮想説明 | 21 | ✅ | L3 |
| 5 | geo-audit-tool | 生成AI検索GEO診断 | 構造スコア+競合比較／日本語×業界別+引用シェア時系列 | 26 | ✅ | L3 |
| 6 | ai-platform-template | AI基盤テンプレート ★土台 | LLM自動ルーティング+観測性／FinOps(コスト予測+予算アラート) | 226 | ✅ (2026-10-28) | L3 |
| 7 | proactive-agent-ux | プロアクティブUX | 提案理由+ワンクリック／バンディット提案ランキング(Thompson) | 23 | ✅ | L3 |
| 8 | physical-ai-llm-demo | LLM×シミュレータ | プラン安全性検証／形式的安全仕様エンジン | 32 | ✅ | L3 |

### B/C系

| # | repo | 概要 | 差別化コア | test | CI | 成熟度 |
|---|------|------|-----------|------|----|-------|
| B1 | ax-audit-platform | AX診断→PoC見積自動化 | マーケ5業務プリセット／業界ベンチ偏差値+ROI感度分析 | 24 | ✅ | **L3未達（公開URLが404）** |
| B2 | ai-advisor-os | AI顧問OS 1人50社 | 顧問承認フロー／トリアージ(リスク×価値で50社優先順位) | 26 | ✅ | L3 |
| B3 | d-airs-dashboard | AI定着スコアD-AIRS | 定着継続計測／将来予測+離脱早期予兆 | 23 | ✅ | L3 |
| B4 | matching-agent-engine | 対話型マッチング(アダプタ式) | 実在IDのみ／成約寄与条件uplift→広告ターゲティング | 22 | ✅ | L3 |
| C1 | dev-agent-platform | 開発エージェント生産性基盤 | AI寄与率計測／因果効果推定+ゲーミング検知 | 31 | ✅ | L3 |

> **未検証（推測で断定しないこと）**: 上記の統計的手法（合成対照法／DID／プラセボ検定／Thompson
> sampling／uplift／因果効果推定）は、コードの存在は確認済みだが **手法の妥当性（推定量の一致性・
> 多重比較補正・サンプルサイズ要件）の第三者レビューは未実施**。顧客に数値を提示する案件
> （#3・B3・C1）は統計レビューを先行させ、それまでは「参考値」表記を必須とする。

### 追加案件（2026-07-24〜）

| # | repo | 概要 | 差別化 | 成熟度 |
|---|------|------|--------|-------|
| A1 | corral | Claude Code/Codex 複数エージェントを1司令塔から統率するWebオーケストレーター | 海外7ツールを調査し**設計思想を選択的に取り込み**、日本語UI/Chatwork/セキュリティ既定で差別化 | L3（テスト・CIは PR #1 で追加中、LICENSE 無し） |

---

## 3. A1 corral 詳細

- GitHub https://github.com/hiroshi57/corral / ローカル `C:/Users/hiroshi_takizawa/corral` / デモ https://corral-tau.vercel.app
- 構成: server(Express+ws, TypeScript) + web(Vite+React)
- **ターゲット**: Claude Code/Codex を業務導入し始めた開発チーム・受託/SIer
- **課題**: ①並行運用の煩雑さ ②レビュー/承認の属人化 ③ローカル実行のセキュリティ不安 ④海外ツールは英語UI
- **収益仮説**: 個人=OSS無償 → チーム/企業向け有償（SSO・監査ログ・リモート接続・共有ワークスペース）
  - ⚠ **前提未成立**: corral に LICENSE ファイルが無く、公開リポジトリは既定で全権利保留＝**OSS ではない**。
    オープンコアを採るなら先に LICENSE を決める（例: コア Apache-2.0 / エンタープライズ機能 BUSL-1.1）
- ※本体はローカル常駐デーモン。Vercel は**ブラウザ内デモ**（VITE_CORRAL_DEMO=1）で UI/UX 確認用。実CLI起動は行わない

### 差別化の主張と、その成立範囲（外部提示時はセットで記載）

| 主張 | 成立する範囲 | 成立しない範囲 |
|------|-------------|---------------|
| Host ヘッダ検証で DNS リバインディング遮断 | ブラウザ経由・ループバック運用時 | 非ブラウザの攻撃者は Host を自由に設定でき無効。CORRAL_HOST=0.0.0.0 では正規の社内FQDNが弾かれる |
| x-corral-token で CSRF 遮断 | 単純なクロスサイト POST | XSS・悪性拡張・同端末の別プロセスからは奪取可能 |
| プロンプトを argv 非経由 | **コマンドインジェクション**は構造的に回避 | **プロンプトインジェクションは防げない**。「インジェクション不可」は誤り |
| ガードレールで危険コマンドをブロック | 指示文の正規表現一致 | 実行時のコマンド傍受ではないため言い換えで回避可能（ブロックリストの原理的限界） |
| 監査ログのハッシュチェーン改ざん検知 | チェーン生成 | **検証(verify)が未実装**。再起動で prevHash が genesis に戻り区間跨ぎ検証が不能 |
| マルチテナント（案件単位分離） | REST API | WebSocket 層は未修正時 分離されない（PR #1 で対応中） |

### セキュリティ修正 PR（2026-10-01 起票・**未マージ**）

https://github.com/hiroshi57/corral/pull/1 — CI は SUCCESS。main には**未反映**。
コード実査で検出した以下を修正している。マージされるまで main は脆弱なまま。

| 内容 | 深刻度 |
|------|--------|
| POST /api/workspaces に権限ゲートが無く、member が自分を owner とする案件を作って権限昇格できる | 高 |
| セッション署名鍵が API トークンと同一。トークンはブラウザへ配布されるため任意ユーザーのセッションを偽造でき RBAC を迂回できる | 高 |
| WebSocket が無認証で、接続直後に全案件のセッション・指示文・ログを配信 | 高 |
| ルート(/) が未認証でオーナートークンを HTML に注入（リモート公開時に閲覧者が owner 化） | 高 |
| dev ログイン既定ON＋新規ユーザーの既定案件への自動 member 参加（出荷 compose は CORRAL_DEMO=0・0.0.0.0 で起動） | 高 |
| 子プロセスへ環境変数を全継承し、API トークン/SAML証明書/Chatworkトークンがエージェントに露出 | 高 |
| pdfjs-dist が GHSA-hq66-cqwq-w95j（悪性PDFで任意JS実行）該当。**PDF の D&D 取込が主機能** | 高 |
| セッション削除を session:create で判定／notify/test が session:view で viewer から外部送信可 | 中 |
| 機密マスキングが sk-ant-… / sk-proj-…（ハイフン入り）を捕捉できない | 中 |

### 未解決（PR #1 の対象外・別PR予定）

| 内容 | 深刻度 |
|------|--------|
| docker サンドボックスが既定 --network none でエージェントCLIがLLM APIに到達できず機能しない。file 受け渡しではホスト側パスを argv に渡すためコンテナ内で解決不能 | 高 |
| 監査ログのハッシュチェーンに検証機能が無い／一覧・エクスポートがメモリ5000件のみ（再起動で UI から消える） | 中 |
| git status --porcelain の固定長スライス解析が rename と日本語ファイル名（core.quotepath）を誤読し保護パス判定を取りこぼす | 中 |
| 差分表示が未追跡ファイルを含めず、直前コミットへフォールバックして無関係なコミットを表示しうる | 中 |
| プロンプト平文を worktree 内 .corral/ に書き、承認時に全追加するため対象リポの .gitignore 次第でコミットに混入 | 中 |

### 実装済み機能（時系列）

- 2026-07-24 最優先5: 通知(Chatwork/Slack) / FinOps(コスト計測・予算アラート) / 生産性ダッシュボード / マルチテナント / 認証RBAC
- 2026-07-24 P3: マルチリポ / ガードレール / 実行モード抽象(local/docker/ssh) / 自己ホスト(Dockerfile+compose)
- 2026-07-24 大企業要件: 監査ログ+SIEM / SAML 2.0 SSO / ドキュメントD&D→AIタスク分解
- 2026-07-24 非課金: PDF取込 / LLMプランナー / インラインレビュー / 監査ハッシュチェーン
- 2026-07-25 UI/協働: 監査UI / メンバー管理 / セッションリプレイ / タスクDAG依存 / PDF・PPTX取込
- 2026-07-25 グラフ: DAG可視化 / GUIエディタ / 条件付きエッジ / AIグラフ生成 / 横断検索 / エージェント自動検出
- 2026-07-25 運用: プレイブック / ノード自動割当 / 実行キュー上限 / PR自動作成
- 2026-08-04 本番モードE2E: 本番モードで実走（実worktree→実commit, 1 file/27秒）
  - ⚠ **1ファイル1セッションの単発**であり、主張の中心である並列多数エージェント・DAG・queue上限・docker/ssh の実走検証は含まない

---

## 4. 公開URL（Vercel）

> 実体は各リポジトリの静的HTML（サーバーサイド処理・実LLM呼び出しは含まない＝L3）。
> #2 tool-calling-agent-mvp はCLIツールのため対象外。

| repo | 本番URL | 2026-11-05 実測 |
|------|---------|----------------|
| dify-rag-approval-agent | https://dify-rag-approval-agent.vercel.app | 200 / 185 KB |
| agent-roi-poc | https://agent-roi-poc.vercel.app | 200 / 181 KB |
| context-rerank-search | https://context-rerank-search.vercel.app | 200 / 193 KB |
| geo-audit-tool | https://geo-audit-tool-delta.vercel.app | 200 / 191 KB |
| ai-platform-template | https://ai-platform-template-ten.vercel.app | 200 / 196 KB |
| proactive-agent-ux | https://proactive-agent-ux.vercel.app | 200 / 190 KB |
| physical-ai-llm-demo | https://physical-ai-llm-demo.vercel.app | 200 / 189 KB |
| **ax-audit-platform** | https://ax-audit-platform.vercel.app | ❌ **404 NOT_FOUND（要再デプロイ／URL要確認）** |
| ai-advisor-os | https://ai-advisor-os-three.vercel.app | 200 / 193 KB |
| d-airs-dashboard | https://d-airs-dashboard.vercel.app | 200 / 183 KB |
| matching-agent-engine | https://matching-agent-engine.vercel.app | 200 / 194 KB |
| dev-agent-platform | https://dev-agent-platform.vercel.app | 200 / 195 KB |
| corral（デモ） | https://corral-tau.vercel.app | 200 / 435 B（SPAシェル）+ JSバンドル |

> 旧記載「全12本 200 OK確認済み（2026-07-17）」は現時点では成立しない（**11/12**）。
> URL の生存は劣化するため、**提案書に載せる前に必ず当日再確認する**こと。

---

## 5. 検証ログ（再現コマンド付き）

| 日付 | 対象 | 方法 | 結果 |
|------|------|------|------|
| 2026-11-05 | 公開URL 13本 | curl で status と size を実測 | 12/13 が 200、ax-audit-platform のみ 404 |
| 2026-11-05 | test 関数数 | grep で `def test_` を全 .py から計数 | 21〜226（7月時点から増加。旧 test21〜32 表記は古い） |
| 2026-11-05 | CI 実績 | gh run list で最新 run を取得 | 11/13 success、dify は failure、tool-calling は run 0 |
| 2026-11-05 | corral PR #1 | gh pr view で state と checks を取得 | state=OPEN / checks=SUCCESS / **未マージ** |
| 2026-10-01 | corral セキュリティ | server/src 実コード精査 + npm audit | 権限昇格・WS無認証・秘密継承・脆弱依存を検出（PR #1） |

### 未検証項目

- docker / ssh 実行モードの実走（インフラ未整備。コードのみ）
- 統計手法の妥当性（第三者レビュー未実施）
- 顧客側の受容性・価格感度（`revenue-refutation.md` 参照）
- Vercel 公開ページが「実検索」を名乗る範囲（クライアント内処理か実API呼び出しかは未確認）

---

## 6. 次アクション（優先順）

1. corral PR #1 をレビュー・マージ（main が脆弱なまま放置されている）
2. ax-audit-platform の公開URL を復旧（提案書に載っている場合は差し替え）
3. dify-rag-approval-agent の CI 失敗を解消（102テストあるのに緑でない）
4. tool-calling-agent-mvp に CI を設置（13本中1本だけ未設置）
5. corral の LICENSE を決定（OSS 前提の収益仮説の前提条件）
6. 統計手法の第三者レビュー（#3・B3・C1 を顧客提示する前に）

---

## 7. 公開リポジトリとしての注意

本リポジトリは Public。ローカル絶対パス・Vercel scope 名・社内ロードマップ・顧客仮説・価格戦略が
すべて公開されている。非公開情報を含めるなら Private 化、または顧客/価格に関する記述を分離すること。
