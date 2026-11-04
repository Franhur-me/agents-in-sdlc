---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot をクラウドで非同期に動作させます。GitHub 上で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

この一連の演習では、cloud agent が従う custom instruction を追加してから、GitHub issue を割り当てて実装を依頼します。custom agent を確認して使い、agents dashboard からセッションの進捗を監視・案内し、最後にプルリクエストをレビューして結果を調整します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を GitHub Copilot cloud agent に割り当てる |
| [3. Custom agent][ex3] | 特化エージェント | custom agent を確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して案内する |
| [5. 反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を調整して次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランをお持ちでない場合、認定学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの環境の演習も完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Free を除く Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この演習では MCP を扱いません。Copilot で MCP server を使う方法については、[CLI 環境](../cli/)または [VS Code 環境](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
