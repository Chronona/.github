# .github

このリポジトリは、GitHub の **default community health files**（既定のコミュニティ
ヘルスファイル）を置くための専用リポジトリです。ここに置いたファイルは、
同じオーナーが持つ他のリポジトリのうち **自前の対応ファイルを持たないもの** に
自動的に適用されます。

> 参考（一次ソース）
> - [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
> - [Syntax for issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)
> - [Syntax for GitHub's form schema](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-githubs-form-schema)

## 収録内容

| パス | 用途 |
| --- | --- |
| `.github/ISSUE_TEMPLATE/bug_report.yml` | バグ報告（再現手順 / 期待する挙動 / 実際の挙動 / 環境情報 / 追加情報） |
| `.github/ISSUE_TEMPLATE/feature_request.yml` | 機能要望（解決したい課題 / 提案する解決策 / 検討した代替案 / 追加情報） |
| `.github/ISSUE_TEMPLATE/task.yml` | 作業タスク（目的 / 完了条件チェックリスト / 補足） |
| `.github/ISSUE_TEMPLATE/config.yml` | テンプレート選択画面の設定（`blank_issues_enabled: true`） |

## 上書き挙動（重要）

- 既定ファイルは **リポジトリ固有のファイルより弱い**。適用先リポジトリに同じ
  ファイルがあれば、そちらが使われます。
- Issue テンプレートについては、上書きは **`ISSUE_TEMPLATE` フォルダ単位の
  オールオアナッシング** です。適用先が自前の
  `.github/ISSUE_TEMPLATE/` を持ち、その中にテンプレートが 1 つでもあると、
  このリポジトリのテンプレートは **1 つも使われません**。
  「不足分だけここから補う」というマージは行われません。
- したがって、適用先で 1 種類だけ独自テンプレートを追加したい場合は、
  **必要な全テンプレートをその適用先にコピーする** 必要があります。
- 既定ファイルはリポジトリにコミットされるものではなく、GitHub が UI 上で
  参照するだけです。適用先のワーキングツリーにはファイルは現れません。

## 編集時の注意

### 1. このリポジトリは Public

閲覧は全世界に公開されます。説明文・プレースホルダ・`value` に、
固有名詞、社内情報、実在のホスト名・URL、個人情報を書かないこと。
汎用的な文言のみを使います。

### 2. `required: true` に依存しない設計にする

Issue Form の入力必須化（`validations.required`）は **Public リポジトリでのみ
有効** です。Private リポジトリに既定として適用された場合、フォームの
バリデーションは効かず、実質すべての項目が任意扱いになります。

そのため本リポジトリのテンプレートは:

- 必須にしたい項目は **label 文言に「(必須)」と明記** する
- そのうえで `validations.required` も併記する（Public では実際に効く）
- **未記入でも起票が成立する前提** で設計する（後から質問して補える文面にする）

という方針を取っています。この方針は変更しないでください。

### 3. `labels:` は使わない

テンプレートの `labels:` が指定するラベルは、**適用先リポジトリに存在していないと
機能しません**。既定テンプレートは多数のリポジトリに適用されるため、
すべての適用先で同じラベルが定義されている保証がありません。
ラベル付与は運用側（起票後のトリアージ）に委ねます。

### 4. `assignees:` は使わない

担当者は適用先リポジトリごとに異なるため、既定テンプレートでは指定しません。

### 5. 変更したら YAML を検証する

パースとスキーマ必須キー（`name` / `description` / `body`、各 body 要素の
`type` / `attributes`）の確認を行ってからコミットしてください。

```sh
# パース確認
for f in .github/ISSUE_TEMPLATE/*.yml; do yq -e '.' "$f" > /dev/null && echo "OK $f"; done
```

なお GitHub 側でも、テンプレートに構文エラーがあるとそのテンプレートは
選択肢に表示されなくなります。壊れても即座に気づけるとは限らないため、
コミット前の検証を習慣にしてください。

## 運用メモ

- 決定事項を残す場合は ADR として別途記録し、作業の経緯は Git 履歴を参照する
  方針です。この README は「現時点の仕様と手順書」として常に最新化します。
