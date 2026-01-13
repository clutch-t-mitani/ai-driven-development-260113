# 02. アーキテクチャ設計

## レイヤ方針（シンプル版）
- Controller：HTTP入出力（Request/Response、リダイレクト、View返却）
- UseCase：業務処理（1UseCase=1業務）
- Model：データ構造/リレーション/スコープ中心（ロジック最小）
- Policy：認可
- FormRequest：入力検証
- View（Blade）：表示のみ（複雑なロジックは避ける）

## ディレクトリ例
- app/Http/Controllers
- app/Http/Requests
- app/Policies
- app/UseCases
  - Post/
  - Comment/
  - Favorite/
  - Tag/
  - Auth/（必要なら）
- app/Models

## UseCase設計ルール
- ControllerはUseCaseを呼ぶだけ（可能な限り）
- UseCaseはトランザクション境界を持つ（必要な場合のみ）
- UseCaseは戻り値として「画面に必要な最小データ」を返す（Eloquent Model/DTOどちらでも可）
- DBアクセスはEloquentで良い（Repository層はMVPでは作らない）

## 例：Controllerの責務
- FormRequestを受ける
- 認可（authorize）を通す
- UseCaseを実行
- 成功：redirect/view
- 失敗：例外 or エラーをセッションに積む

## 例外/エラー方針
- 想定内エラー：バリデーション、認可、対象が存在しない（404）
- 予期せぬエラー：ログに出し、ユーザには汎用メッセージ

## パフォーマンス方針
- 一覧は eager load を基本（with）
- カウント系（favorites_count/comments_count）は withCount を基本
- ページネーションは必須
- 検索はまずDB LIKE（将来 Scout + Meilisearch へ差し替え）
