# 業務ツール事業 マスター索引（task-lists.md）

最終更新: 2026-08-21 ／ このファイルが全案件の入口です。別PCからはこのファイルを見れば全体を把握できます。

---

## 0. 最初に読む要点

| 項目 | 内容 |
|---|---|
| **何を作っているか** | 入力するだけで計算する業務ツール群。**LLM APIを使わない**（キー不要・オフライン動作） |
| **なぜ売れるか（唯一の差別化）** | **機密情報を外に出さずに処理できる**。ChatGPTに貼れないデータを扱える。完全オフライン動作を実測で確認済み |
| **課金方式** | 買い切りライセンス。試用版は印刷物に「試用版」透かし＋サーバー共有不可 |
| **現在の到達点** | 19本が本番稼働・GitHub反映済み。**ただし顧客に提示した実績は0件**（売れる保証はない） |
| **次にやること** | 価格ページ／キー発行ツール／新規10本（下記 §5, §6） |

---

## 1. 完成済み 19本（すべて本番稼働・hiroshi57 に push 済み）

### 全19本に共通で実装済みの機能
- 実データ入力 → 実計算（あらかじめ用意した答えは出さない）
- 入力の自動保存（localStorage）＋ JSON書き出し／読み込み／リセット
- **完全オフライン動作**（外部CDN依存ゼロ・起動時通信0件を実測）
- 印刷／PDF保存（提案書に添付できる体裁）＋ 結果をExcelに貼れるコピー
- サーバー保管・複数人共有（閲覧用リンク／編集用リンク・更新履歴）
- 試用版／正規版のライセンス切替（会社名＋キーをオフライン検証）
- 提供者・免責フッター／利用規約・プライバシーポリシーへのリンク／OGP・ファビコン

### 一覧

| # | ツール名 | 本番URL | GitHub | 主な用途 |
|---|---|---|---|---|
| 1 | GEO/LLMO診断 | https://geo-audit-tool-delta.vercel.app | hiroshi57/geo-audit-tool | URLを入れて生成AIに引用されやすい構造かを実解析・競合比較 |
| 2 | AX診断 | https://ax-audit-platform.vercel.app | hiroshi57/ax-audit-platform | 業務を入力→AI導入の優先度とROIを実計算 |
| 3 | AI導入ROI検証 | https://agent-roi-poc.vercel.app | hiroshi57/agent-roi-poc | PoC実測値→年間効果・回収・導入可否 |
| 4 | AI定着度（D-AIRS） | https://d-airs-dashboard.vercel.app | hiroshi57/d-airs-dashboard | 4ドメイン・部署別で定着度を可視化 |
| 5 | AI顧問OS | https://ai-advisor-os-three.vercel.app | hiroshi57/ai-advisor-os | クライアントのAI活用度を事実ベースで採点 |
| 6 | AI効果チェック（開発） | https://dev-agent-platform.vercel.app | hiroshi57/dev-agent-platform | AI利用有無で作業日数を比較・ゲーミング検知 |
| 7 | 賃貸物件マッチング | https://matching-agent-engine.vercel.app | hiroshi57/matching-agent-engine | 条件→在庫の実照合・緩和シミュレーション |
| 8 | 言い換え対応 社内文書検索 | https://context-rerank-search.vercel.app | hiroshi57/context-rerank-search | BM25＋同義語展開。「キャンセル」で「解約」を探す |
| 9 | 社内規程AI＋承認 | https://dify-rag-approval-agent.vercel.app | hiroshi57/dify-rag-approval-agent | 規程を実検索し条文を引用。無ければ答えない |
| 10 | AIコスト管理 | https://ai-platform-template-ten.vercel.app | hiroshi57/ai-platform-template | 用途別トークン試算・最適モデル配分 |
| 11 | 先回り通知 | https://proactive-agent-ux.vercel.app | hiroshi57/proactive-agent-ux | 締切・金額から優先度を理由つきで判定 |
| 12 | ロボット安全確認 | https://physical-ai-llm-demo.vercel.app | hiroshi57/physical-ai-llm-demo | 経路探索を実行し危険なら実行を止める |
| 13 | 会議コスト可視化 | https://meeting-cost-visualizer.vercel.app | hiroshi57/meeting-cost-visualizer | 会議の年間コストと削減額を実計算 |
| 14 | 議事録チェッカー | https://minutes-quality-checker.vercel.app | hiroshi57/minutes-quality-checker | 決定事項・担当・期限の抜け漏れ検出 |
| 15 | アジェンダ設計 | https://agenda-designer.vercel.app | hiroshi57/agenda-designer | 目的から時間配分と案内文を生成 |
| 16 | 動画構成プランナー | https://video-script-planner.vercel.app | hiroshi57/video-script-planner | 秒単位の構成表とフック設計 |
| 17 | A/Bテスト設計 | https://creative-abtest-planner.vercel.app | hiroshi57/creative-abtest-planner | 必要サンプル数・検証日数を統計式で算出 |
| 18 | 広告クリエイティブ疲弊チェック | https://creative-fatigue-detector.vercel.app | hiroshi57/creative-fatigue-detector | 日別CTRから差し替え時期を判定 |
| 19 | 表データ品質チェック | https://data-quality-checker-henna.vercel.app | hiroshi57/data-quality-checker | 空欄・重複・全角・日付混在・外れ値を検出 |

> **URL注意**: geo / ai-advisor / ai-platform / data-quality は clean alias が他アカウントに取得済みのため `-delta` `-three` `-ten` `-henna` が付きます。デプロイ後は必ず `vercel project ls` で正式URLを確認してください。

---

## 2. 共通基盤

| 種別 | 内容 | 場所 |
|---|---|---|
| 共有API | サーバー保管・複数人共有 | https://di-tools-api.vercel.app/api/workspace ／ ローカル `di-tools-api/` |
| 保管領域 | Vercel Blob（private・米国 iad1） | ストア名 `di-ws-main` |
| 法務文書 | 利用規約・プライバシーポリシー | https://di-tools-legal.vercel.app/terms.html ／ `/privacy.html` ／ ローカル `di-tools-legal/` |
| ライセンス | キー生成・検証ロジック（共通） | `license_core.js` |

### 共有APIの使い方
```
POST   /api/workspace              新規作成 → {id, editKey}
GET    /api/workspace?id=          読み込み（閲覧は誰でも）
PUT    /api/workspace?id=&key=     上書き保存（編集キー必須）
DELETE /api/workspace?id=&key=     削除
```

---

## 3. 作業用スクリプト（別PCでも同じ手順が再現できます）

| スクリプト | 役割 |
|---|---|
| `app_template.js` | 新規ツールの外殻を生成（品質基準を内蔵） |
| `make_offline.js` | 外部CDN依存を除去し1ファイルで自己完結させる |
| `inject_commercial.js` | 印刷CSS・結果コピー・免責フッター・OGPを全ツールへ注入 |
| `inject_share.js` | サーバー保管・共有機能を全ツールへ注入 |
| `inject_legal.js` | 規約・プライバシーポリシーへのリンクを注入 |
| `inject_license.js` | 試用版／正規版のライセンス機能を注入 |
| `verify_offline.js` | **通信を遮断した状態で全ツールが動くか検証** |
| `verify_license.js` | 試用版→正規版の切替・不正キー拒否を検証 |
| `verify_new.js` | 実描画・保存・復元・説明/誘導/出所の有無を検証 |
| `audit_commercial.js` | 外販要件（印刷・コピー・免責・OGP・NaN・入力耐性）を監査 |
| `test_share_e2e.js` | Aさん保存→Bさん別端末で閲覧、の通しテスト |
| `vendor/react.js`, `vendor/react-dom.js`, `bt.js` | オフライン化に使う同梱ライブラリ |

### 別PCで作業を再開する手順
```bash
# 1. 必要なリポジトリを clone（例）
gh repo clone hiroshi57/meeting-cost-visualizer

# 2. 検証用ライブラリを用意
mkdir -p vendor
curl -sL -o vendor/react.js     https://unpkg.com/react@18.3.1/umd/react.production.min.js
curl -sL -o vendor/react-dom.js https://unpkg.com/react-dom@18.3.1/umd/react-dom.production.min.js
curl -sL -o bt.js               https://unpkg.com/@babel/standalone@7.25.6/babel.min.js
npm install jsdom --no-save

# 3. 検証を実行
node verify_offline.js
node verify_license.js
```

---

## 4. 必ず守るルール（過去に事故を起こした項目）

| ルール | 理由（実際に起きたこと） |
|---|---|
| **`gh auth setup-git` を実行しない** | Windowsの資格情報が別アカウントのトークンで上書きされ、全リポジトリのpushが403になった。復旧には `gh auth login` での再認証が必要だった |
| **pushの成否は `git rev-list --count origin/main..HEAD` で確認する** | `git push -q \| tail -1` の後に `$?` を見ると *tail* の結果を見てしまい、失敗しても成功と表示された。実際に19本すべてが未pushのまま「完了」と報告してしまった |
| **`String.replace` の置換文字列に `$&` が入る可能性がある場合は関数形式にする** | JSX内の `replace(/../g,"\\$&")` が展開され、2本のファイルが破損した |
| **デプロイ後は `vercel project ls` で正式URLを確認する** | clean alias が他アカウントに取られており、別プロジェクトを見て「未反映」と誤判断した |
| **`vercel blob create-store` は `--yes` を付ける** | 対話プロンプトで固まる。`yes \|` は無限ループして342MBの出力を出した |
| **オフライン化後は `verify_all.js` ではなく `verify_offline.js` を使う** | JSXを事前変換したため、旧テストは「JSXブロックなし」で必ず失敗する |

---

## 5. 未完了タスク（優先順）

| # | タスク | 状態 | 備考 |
|---|---|---|---|
| 1 | **価格ページ `pricing.html`** | ❌ 未着手 | アプリ内からリンク済みなので**現在リンク切れ**。最優先 |
| 2 | **ライセンスキー発行ツール** | ❌ 未着手 | 購入者へキーを発行する手段。`license_core.js` を使うローカルHTML |
| 3 | 決済（Stripe）との接続 | 未着手 | 決済はユーザー側に既存。購入→キー発行の導線を繋ぐ |
| 4 | ポータル（19本の一覧・入口） | 保留 | 「確実に売れる状態になってから」との方針。§6の検証後 |
| 5 | アクセス解析タグの埋め込み | 未着手 | 解析ツールはユーザー側に既存。IDを受領後に埋め込む |
| 6 | 新規10本の開発 | 🔄 着手予定 | §6 |
| 7 | 顧客への提示・価格の反応確認 | ❌ 未実施 | **「売れる」の唯一の証拠。これが無い限り売れる保証はない** |

---

## 6. 新規10本の企画（売れる可能性が高いもの）

### 選定基準（既存19本の敵対的検証から導いた条件）
既存19本の弱点は「ChatGPTで代替できる」「無料の定番競合がいる」ことでした。そこで**次の4条件を満たすもの**に絞ります。

1. **機密情報を扱う** → ChatGPTに貼れない ＝ オフラインの価値が出る
2. **法令・制度が絡む（日本固有）** → 海外の無料ツールが存在しない
3. **成果物（点検記録・報告書）が必要** → 印刷透かしのゲートが効く
4. **繰り返し使う** → 買い切りライセンスが売れる

### 一覧

| # | repo（予定） | ツール名 | 解決する課題 | 4条件 |
|---|---|---|---|---|
| 1 | `pii-scrubber` | **個人情報 混入チェッカー** | ChatGPTに貼る前に、氏名・電話・メール・住所・マイナンバー等が混ざっていないかを検出しマスキング | ①②③④ すべて |
| 2 | `overtime-compliance` | 残業時間 法令チェック | 36協定の上限（月45h/年360h、特別条項）に対する超過を判定 | ①②③④ |
| 3 | `invoice-matcher` | 発注・請求 突合 | 発注データと請求データを照合し金額・数量の不一致を検出 | ①③④ |
| 4 | `contract-expiry-manager` | 契約期限・更新管理 | 契約書の期限を一覧化し、自動更新・解約通知期限を警告 | ①③④ |
| 5 | `customer-list-merger` | 顧客名簿 名寄せ | 表記ゆれ（全角半角・法人格・空白）を吸収して重複を統合 | ①③④ |
| 6 | `shift-legal-checker` | シフト法令チェック | 連続勤務日数・休憩時間・インターバルの適法性を判定 | ①②③④ |
| 7 | `invoice-qualified-checker` | 適格請求書（インボイス）チェック | 登録番号の形式・必要記載事項の欠落を検出 | ①②③④ |
| 8 | `price-passthrough` | 値上げ交渉 根拠計算 | 原価上昇率から必要な転嫁率と交渉根拠資料を作成 | ①③④ |
| 9 | `subsidy-eligibility` | 補助金 要件自己診断 | 要件を分解し、該当可否と不足要件を提示 | ②③④ |
| 10 | `security-checklist` | AI利用 セキュリティ自己点検 | 社内AI利用のリスクを点検し、情報システム部提出用の記録を出力 | ②③④ |

> **最有望は #1「個人情報 混入チェッカー」**です。「ChatGPTを使う前に必ず通すツール」という位置づけで、AI利用ルールがある企業すべてが対象になります。**ChatGPT自体には絶対にできない**（貼る前のチェックなので）という点で、代替不可能です。

---

## 7. 正直な現状評価

- **技術的な完成度**：実測で検証済み（オフライン動作・ライセンス・共有・印刷すべて19/19合格）
- **商品としての完成度**：価格ページとキー発行がないため、**まだ売れません**
- **売れる保証**：**ありません。** 顧客に価格を提示した実績が0件で、需要は未検証です
- **LLMは使っていません**：APIキーが無効なため。「AI」を期待した顧客との期待値ギャップに注意

### 推奨する進め方
1. §5の1〜2（価格ページ・キー発行）を作る
2. §6の#1（個人情報チェッカー）を作る
3. **既存クライアント2〜3社に #1 と既存3本を提示し、「いくらなら買うか」を聞く**
4. 反応があったものだけをポータルに載せる
