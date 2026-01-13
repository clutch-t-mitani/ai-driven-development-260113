# GitHub Copilot カスタム指示（Laravel 12 & PHP 8.5）

本ファイルは、GitHub Copilot が Laravel 12 および PHP 8.5 を用いた
保守性・可読性・安全性の高いコードを生成するための制約を定義する。

Controller にビジネス処理を記述することは禁止し、
必ず UseCase クラスに処理を切り出すものとする。

---

## 一般的なコーディング規約

- PSR-12 に準拠する
- 可読性と明確さを最優先する
- 意味が明確な命名を行う（省略は禁止）
- 複雑なクラス・メソッドには PHPDoc を記述する
- 単一責任原則（SRP）を遵守する
- マジックナンバーやハードコードされた文字列を使用しない
- 不要な副作用を極力排除する

---

## PHP 8.5 ベストプラクティス

- 可能な限り `readonly` プロパティを使用する
- 定数の代替として Enum を使用する
- First-class callable 構文を使用する
- コンストラクタプロパティプロモーションを活用する
- Union / Intersection Types を適切に使用する
- 戻り値型は必ず厳密に指定する（`true` / `false` / `never` を含む）
- Static Return Type を適切に使用する
- Nullsafe Operator（`?->`）を使用する
- 継承を想定しないクラスは `final` とする
- Named Arguments を用いて可読性を高める
- PHP 8.5 の新機能および非推奨事項を常に考慮する

---

## アーキテクチャ方針（非 DDD）

- Controller にビジネスロジックを記述してはならない
- 業務処理・アプリケーションロジックは UseCase クラスに集約する
- Eloquent Model はデータ構造としてのみ使用する
- UseCase は「1 クラス = 1 業務処理」を原則とする
- Repository パターンの使用は必須ではない（必要な場合のみ使用可）

---

## ディレクトリ構成

```
app/
  UseCases/
    User/
      CreateUserUseCase.php
      UpdateUserUseCase.php

  DTOs/
    User/
      CreateUserInput.php

  Http/
    Controllers/
    Requests/
    Resources/

  Models/
```
---

## レイヤ責務

Controller（Presentation）
- HTTP リクエスト／レスポンスの変換のみを行う
- バリデーション済み Request を UseCase に渡す
- ビジネスロジックを含めてはならない
- Model を直接操作してはならない
- 業務的な条件分岐を持ってはならない

UseCase
- 業務処理・アプリケーションロジックを担当する
- Controller から呼び出される唯一の処理単位とする
- Eloquent / DB / 外部 API を直接扱ってよい
- 処理は明確に分離し、肥大化させない

Model（Eloquent）
- データ構造およびリレーション定義に専念する
- 複雑な業務ロジックを記述してはならない
- Scope / Accessor の使用は許可する
---
