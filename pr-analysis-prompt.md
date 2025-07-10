# GitHub PR分析プロンプト - AI駆動開発の段階的効果測定

## Claude Code用 PR分析プロンプト（AI導入段階別）

以下のプロンプトをClaude Codeにコピー＆ペーストして使用してください。

---

```
GitHubリポジトリのPR分析を実施し、AI導入の各段階における効果を測定してください。

【前提条件】
- GitHub CLI (gh) がインストール済み
- 対象リポジトリ: [OWNER/REPO] ※実際のリポジトリ名に置き換えてください

【分析期間の定義】
以下の期間を正確に入力してください（期間の長さがバラバラでも問題ありません）：

1. AI導入前: [YYYY-MM-DD] 〜 [YYYY-MM-DD] (X日間)
2. ChatGPT利用期: [YYYY-MM-DD] 〜 [YYYY-MM-DD] (Y日間)  
3. GitHub Copilot利用期: [YYYY-MM-DD] 〜 [YYYY-MM-DD] (Z日間)
4. Cursor利用期: [YYYY-MM-DD] 〜 [YYYY-MM-DD] (W日間)
5. Claude Code利用期: [YYYY-MM-DD] 〜 [YYYY-MM-DD] (V日間)

【実行手順】

1. 各期間のPRデータを取得してください：
```bash
# 1. AI導入前
gh pr list --repo [OWNER/REPO] --state merged --limit 1000 \
  --search "merged:[開始日]..[終了日]" \
  --json number,title,url,author,additions,deletions,mergedAt,createdAt \
  > prs_no_ai.json

# 2. ChatGPT利用期
gh pr list --repo [OWNER/REPO] --state merged --limit 1000 \
  --search "merged:[開始日]..[終了日]" \
  --json number,title,url,author,additions,deletions,mergedAt,createdAt \
  > prs_chatgpt.json

# 3. GitHub Copilot利用期
gh pr list --repo [OWNER/REPO] --state merged --limit 1000 \
  --search "merged:[開始日]..[終了日]" \
  --json number,title,url,author,additions,deletions,mergedAt,createdAt \
  > prs_copilot.json

# 4. Cursor利用期
gh pr list --repo [OWNER/REPO] --state merged --limit 1000 \
  --search "merged:[開始日]..[終了日]" \
  --json number,title,url,author,additions,deletions,mergedAt,createdAt \
  > prs_cursor.json

# 5. Claude Code利用期
gh pr list --repo [OWNER/REPO] --state merged --limit 1000 \
  --search "merged:[開始日]..[終了日]" \
  --json number,title,url,author,additions,deletions,mergedAt,createdAt \
  > prs_claude_code.json
```

2. 期間の長さが異なるため、以下の正規化した指標も計算してください：
- 日次平均PR数 = 総PR数 ÷ 期間日数
- 週次平均PR数 = 日次平均PR数 × 7
- 月次換算PR数 = 日次平均PR数 × 30
- 一人あたり日次平均PR数 = 日次平均PR数 ÷ アクティブ開発者数

3. 以下の形式でレポートを作成してください：

## AI駆動開発 段階別効果測定レポート

### エグゼクティブサマリー
| AI導入段階 | 期間 | 日数 | 総PR数 | 日次平均 | 月次換算 | 前段階比 |
|-----------|------|------|--------|----------|----------|----------|
| AI導入前 | MM/DD-MM/DD | X日 | XXX | X.X | XXX | - |
| ChatGPT | MM/DD-MM/DD | Y日 | XXX | X.X | XXX | +XX% |
| Copilot | MM/DD-MM/DD | Z日 | XXX | X.X | XXX | +XX% |
| Cursor | MM/DD-MM/DD | W日 | XXX | X.X | XXX | +XX% |
| Claude Code | MM/DD-MM/DD | V日 | XXX | X.X | XXX | +XX% |

**主要な発見:**
- 最も効果が高かったツール: [ツール名] (日次平均 +XX%)
- 累積効果: AI導入前と比較して日次平均 +XX%
- 投資対効果が最も高いツール: [分析結果]

### 段階別PR一覧

#### 1. AI導入前（[期間]）
| PR番号 | タイトル | 作成者 | 変更行数 | 作成→マージ | リンク |
|--------|---------|--------|----------|-------------|--------|
| #123 | feat: ユーザー認証機能 | @user1 | +150/-30 | 3.5日 | [Link](URL) |
（主要なPRを10-20件程度リスト）

#### 2. ChatGPT利用期（[期間]）
| PR番号 | タイトル | 作成者 | 変更行数 | 作成→マージ | リンク |
|--------|---------|--------|----------|-------------|--------|
| #234 | feat: API最適化 | @user2 | +200/-50 | 2.0日 | [Link](URL) |
（主要なPRを10-20件程度リスト）

[以下、各段階同様にリスト]

### 作成者別・段階別統計

| 作成者 | AI導入前 | ChatGPT | Copilot | Cursor | Claude Code | 最大向上率 |
|--------|----------|---------|---------|--------|-------------|------------|
| | PR/月 | PR/月 | PR/月 | PR/月 | PR/月 | (vs AI導入前) |
| @user1 | 5.0 | 6.5 | 8.0 | 10.5 | 15.0 | +200% |
| @user2 | 4.0 | 4.5 | 7.0 | 9.0 | 12.0 | +200% |
| @user3 | 3.0 | 3.5 | 5.0 | 8.0 | 11.0 | +267% |
（全アクティブ開発者分）

### 詳細分析

#### 生産性指標の推移
| 指標 | AI導入前 | ChatGPT | Copilot | Cursor | Claude Code |
|------|----------|---------|---------|--------|-------------|
| 平均PR/人/週 | X.X | X.X | X.X | X.X | X.X |
| 平均変更行数/PR | XXX | XXX | XXX | XXX | XXX |
| PR作成→マージ時間 | X.X日 | X.X日 | X.X日 | X.X日 | X.X日 |
| コードレビュー時間 | X.X時間 | X.X時間 | X.X時間 | X.X時間 | X.X時間 |

#### ツール別の特徴的な変化
**ChatGPT導入時:**
- 主な変化: [PRタイトルやコミットメッセージから推測される変化]
- 効果が高かった領域: [バックエンド/フロントエンド/テストなど]

**GitHub Copilot導入時:**
- 主な変化: コード補完による実装速度向上
- 特に効果的だった開発者: [@user1, @user2]

**Cursor導入時:**
- 主な変化: より大きな機能単位での実装
- PRサイズの変化: [分析結果]

**Claude Code導入時:**
- 主な変化: 複雑な機能の一括実装
- 新規参入開発者の生産性: [分析結果]

### コスト効果分析
| ツール | 月額コスト | 生産性向上 | ROI | 推奨度 |
|--------|-----------|-----------|-----|--------|
| ChatGPT | $20/人 | +XX% | XX倍 | ★★★ |
| Copilot | $10/人 | +XX% | XX倍 | ★★★★ |
| Cursor | $20/人 | +XX% | XX倍 | ★★★★ |
| Claude Code | $XX/人 | +XX% | XX倍 | ★★★★★ |

### 推奨事項
1. 段階的導入の推奨順序
2. チーム規模別の最適なツール選択
3. 投資対効果を最大化する組み合わせ

4. 追加の分析コマンド：
```bash
# 各期間のアクティブ開発者数を確認
jq '[.[] | .author.login] | unique | length' prs_no_ai.json
jq '[.[] | .author.login] | unique | length' prs_chatgpt.json
# ... 各ファイルで実行

# PRの作成からマージまでの時間を分析（生産性指標）
jq '.[] | {
  number: .number,
  title: .title,
  author: .author.login,
  created: .createdAt,
  merged: .mergedAt,
  hours_to_merge: (((.mergedAt | fromdateiso8601) - (.createdAt | fromdateiso8601)) / 3600)
}' prs_claude_code.json | jq -s 'sort_by(.hours_to_merge)'

# 期間を正規化した比較（日次平均の計算）
echo "AI導入前: $(jq length prs_no_ai.json) PRs ÷ X日 = 日次平均"
echo "ChatGPT: $(jq length prs_chatgpt.json) PRs ÷ Y日 = 日次平均"
# ... 各段階で計算
```

【分析の注意事項】
- 期間の長さが異なるため、必ず正規化（日次/週次/月次換算）して比較
- 季節要因（年末年始、夏季休暇など）を考慮
- チーム人数の変動がある場合は一人あたりの指標で比較
- 各ツールの学習曲線を考慮（導入直後は効果が低い可能性）

このレポートをMarkdownファイルとして出力し、どのAIツールがどの段階でどの程度効果があったかを明確に示してください。
```

---

## 期間正規化の計算方法

### 基本的な正規化指標

```python
# Claude Codeに計算してもらうPythonスクリプト例
import json
from datetime import datetime

def analyze_period(filename, period_name, start_date, end_date):
    with open(filename, 'r') as f:
        prs = json.load(f)
    
    # 期間の日数を計算
    start = datetime.fromisoformat(start_date)
    end = datetime.fromisoformat(end_date)
    days = (end - start).days + 1
    
    # 基本統計
    total_prs = len(prs)
    unique_authors = len(set(pr['author']['login'] for pr in prs))
    
    # 正規化指標
    daily_avg = total_prs / days
    weekly_avg = daily_avg * 7
    monthly_avg = daily_avg * 30
    per_person_daily = daily_avg / unique_authors if unique_authors > 0 else 0
    
    # 変更行数の統計
    total_changes = sum(pr['additions'] + pr['deletions'] for pr in prs)
    avg_changes_per_pr = total_changes / total_prs if total_prs > 0 else 0
    
    return {
        'period': period_name,
        'days': days,
        'total_prs': total_prs,
        'unique_authors': unique_authors,
        'daily_avg': round(daily_avg, 2),
        'weekly_avg': round(weekly_avg, 2),
        'monthly_avg': round(monthly_avg, 2),
        'per_person_daily': round(per_person_daily, 2),
        'avg_changes_per_pr': round(avg_changes_per_pr, 2)
    }

# 各期間を分析
periods = [
    ('AI導入前', 'prs_no_ai.json', '2023-01-01', '2023-03-31'),
    ('ChatGPT', 'prs_chatgpt.json', '2023-04-01', '2023-05-15'),
    ('Copilot', 'prs_copilot.json', '2023-05-16', '2023-07-31'),
    ('Cursor', 'prs_cursor.json', '2023-08-01', '2023-10-31'),
    ('Claude Code', 'prs_claude_code.json', '2023-11-01', '2024-01-15')
]

results = []
for period_name, filename, start, end in periods:
    results.append(analyze_period(filename, period_name, start, end))

# 比較表を生成
print("| 期間 | 日数 | 総PR | 日次平均 | 月次換算 | 人/日 |")
print("|------|------|------|----------|----------|-------|")
for r in results:
    print(f"| {r['period']} | {r['days']} | {r['total_prs']} | "
          f"{r['daily_avg']} | {r['monthly_avg']} | {r['per_person_daily']} |")
```

## 視覚的な分析オプション

### グラフ生成プロンプト（追加）

```
取得したデータから以下のグラフを生成してください：

1. **段階別生産性推移グラフ**
   - X軸: AI導入段階
   - Y軸: 月次換算PR数
   - 複数の線: 各開発者の推移

2. **投資対効果グラフ**
   - X軸: 月額コスト（ツール費用）
   - Y軸: 生産性向上率
   - バブルサイズ: 利用者数

3. **PRサイズ分布の変化**
   - 各段階でのPR変更行数のヒストグラム
   - AIツールによってPRサイズがどう変化したか

matplotlib や plotly を使用して可視化してください。
```

## カスタマイズ例

### チーム環境に応じた分析

```
# モバイルチーム向けの分析
- iOS/Androidそれぞれの生産性変化を分離
- Swift/Kotlin ファイルの変更に絞った分析

# バックエンドチーム向けの分析  
- API関連のPRに絞った分析
- データベースマイグレーションの頻度変化

# 新人開発者の成長分析
- 入社1年未満の開発者の生産性向上率
- AIツールによるオンボーディング期間の短縮効果
```

このプロンプトを使用することで、期間の長さが異なる各AI導入段階でも、公平に比較可能な分析結果が得られます。