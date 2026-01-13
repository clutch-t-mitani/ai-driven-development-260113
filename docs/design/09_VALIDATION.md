# 09. バリデーション設計（FormRequest）

原則：入力はFormRequestで統一し、UseCaseに「検証済み入力」を渡す。

## PostStoreRequest / PostUpdateRequest
- title：required, string, max:255
- body：required, string
- status：required, in:draft,published
- tags：nullable（文字列 or 配列）
  - MVPは文字列（例：カンマ区切り）で受けてUseCaseで正規化

追加案：
- 禁止語、過剰な長文制限などは運用で追加

## CommentStoreRequest / CommentUpdateRequest
- body：required, string, max:2000（上限は適宜）

## SearchRequest（任意）
- q：nullable, string, max:255
- tag：nullable, string, max:64
- sort：nullable, in:new,popular

## エラーメッセージ
- MVPはLaravel標準で良い
- 余裕があれば日本語化（lang/ja）
