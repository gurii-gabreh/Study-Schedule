# Study Schedule

学習タスクをレベル1(目標)〜レベル4(実行タスク)のWBS(階層構造)で管理し、1日1列のカレンダー(WBS/ガントチャート形式)で進捗・遅延を可視化するダッシュボード。GitHub Pagesで公開(`.github/workflows/deploy-pages.yml`、mainへのpushで自動デプロイ)。

## 使い方

- **閲覧**: `index.html`が唯一のビューア。`data/tasks.json`をfetchして表示する(データの埋め込みはしない)。
- **タスクの追加・ステータス更新**: このリポジトリを直接編集するのではなく、Claude Codeのチャットで依頼する。「〇〇という目標を追加して」「このタスクを完了にして」のように伝えると、`data/tasks.json`が更新・commitされる。

## データ構造(`data/tasks.json`)

`root`を起点に`children`で再帰的にツリーを構成する。**葉ノード(childrenを持たないノード)だけ**が実行タスク(Lv4)として`start`/`plannedEnd`/`actualEnd`/`status`(未対応・対応中・完了)/`priority`(1〜3)を持つ。それ以外の中間ノード(Lv1〜3)は`children`だけを持ち、開始日・終了日・ステータス・遅延日数・完了数はビューア側(`index.html`)が子タスクから自動集計する。

```json
{
  "root": {
    "title": "定期テストで学年順位を上げる",
    "subtitle": "2学期 中間試験ゴール",
    "summary": "...",
    "children": [
      {
        "title": "英語",
        "children": [
          {
            "title": "リスニング力を強化する",
            "children": [
              {
                "title": "TOEIC公式問題集 Vol.9 リスニング演習",
                "subtitle": "Part3・4 弱点補強",
                "summary": "...",
                "start": "2026-08-16",
                "plannedEnd": "2026-08-20",
                "actualEnd": null,
                "status": "未対応",
                "priority": 2
              }
            ]
          }
        ]
      }
    ]
  }
}
```
