# 04. データベース設計

## テーブル一覧
- users
- posts
- comments
- favorites
- tags
- post_tag

## users
| column | type | null | note |
|---|---|---:|---|
| id | bigint | NO | PK |
| name | varchar | NO | 表示名 |
| email | varchar | NO | unique |
| email_verified_at | datetime | YES | メール認証（オプション） |
| password | varchar | NO | bcrypt |
| remember_token | varchar | YES | |
| created_at/updated_at | datetime | NO | |

## posts
| column | type | null | note |
|---|---|---:|---|
| id | bigint | NO | PK |
| user_id | bigint | NO | FK(users) |
| title | varchar | NO | index推奨 |
| content | text | NO | |
| status | enum | NO | published/draft |
| view_count | bigint | NO | default 0 |
| created_at/updated_at | datetime | NO | |
| deleted_at | datetime | YES | soft delete |

推奨index：
- (user_id)
- (status, created_at)
- (view_count)
- (created_at)

## comments
| column | type | null | note |
|---|---|---:|---|
| id | bigint | NO | PK |
| post_id | bigint | NO | FK(posts) |
| user_id | bigint | NO | FK(users) |
| comment | text | NO | |
| created_at/updated_at | datetime | NO | |
| deleted_at | datetime | YES | soft delete |

推奨index：
- (post_id, created_at)
- (user_id)

## favorites
| column | type | null | note |
|---|---|---:|---|
| id | bigint | NO | PK |
| post_id | bigint | NO | FK(posts) |
| user_id | bigint | NO | FK(users) |
| created_at | datetime | NO | |

制約：
- unique(post_id, user_id)

推奨index：
- unique(post_id, user_id)
- (user_id, created_at)

## tags
| column | type | null | note |
|---|---|---:|---|
| id | bigint | NO | PK |
| name | varchar | NO | 表示名 |
| slug | varchar | NO | unique（URL/検索用） |
| created_at/updated_at | datetime | NO | |

## post_tag
| column | type | null | note |
|---|---|---:|---|
| post_id | bigint | NO | FK(posts) |
| tag_id | bigint | NO | FK(tags) |

制約：
- primary key (post_id, tag_id)

## マイグレーション方針
- まずMVPでは上記を最小実装
- 後で全文検索や通知を追加しやすいよう、postsは拡張余地を残す
