
# AI駆動開発テストプロジェクト

本プロジェクトは、GitHub Copilot を活用した AI 駆動開発の検証および学習を目的としたテストプロジェクトです。

## 使用技術

- **PHP**: 8.5
- **Laravel**: 12
- **アーキテクチャ**: UseCase パターン（非 DDD）
  - Controller は HTTP 層のみを担当
  - ビジネスロジックは UseCase クラスに集約
  - Eloquent Model はデータ構造として使用

## 開発方針

- PSR-12 準拠
- PHP 8.5 の新機能を活用（readonly、Enum、First-class callable など）
- 保守性・可読性・安全性を重視したコード設計
- 単一責任原則（SRP）の徹底

