# Amazon WorkSpaces - Microsoft Office 2024 アプリケーション対応

**リリース日**: 2026 年 1 月 22 日
**サービス**: Amazon WorkSpaces
**機能**: Microsoft Office LTSC 2024 アプリケーション対応

## 概要

Amazon WorkSpaces Personal および Core が、Microsoft Office LTSC Professional Plus 2024、Microsoft Office LTSC Standard 2024、Microsoft Visio LTSC Professional 2024、Microsoft Visio LTSC Standard 2024、Microsoft Project Professional 2024、Microsoft Project Standard 2024 の新しい Microsoft 生産性アプリケーションに対応しました。

これらのアプリケーションは、Amazon WorkSpaces のマネージドアプリケーションカタログに追加され、既存の Manage application ワークフローを使用して新規または既存の WorkSpaces インスタンスに追加できます。これにより、現在のバンドルを変更することなく、モダンで安全な生産性重視のデスクトップ環境を標準化できます。

**アップデート前の課題**

- Microsoft Office 2024、Visio 2024、Project 2024 を WorkSpaces で使用するには、カスタムイメージの作成またはバンドルの移行が必要でした
- 既存の WorkSpaces インスタンスに新しいアプリケーションバージョンを追加する際、ルートボリュームとユーザーボリュームの両方を保持したまま更新することが困難でした
- 組織全体で最新の生産性アプリケーションを標準化するプロセスが複雑でした

**アップデート後の改善**

- Manage application ワークフローを使用して、既存の WorkSpaces インスタンスに Microsoft Office 2024 関連アプリケーションを簡単に追加できるようになりました
- ルートボリュームとユーザーボリュームを保持したまま、アプリケーションバンドルを追加、削除、またはアップグレードできるようになりました
- 現在のバンドル構成を変更することなく、組織全体で最新の生産性アプリケーションを迅速に展開できるようになりました

## サービスアップデートの詳細

### 対応アプリケーション

1. **Microsoft Office LTSC Professional Plus 2024**
   - Word、Excel、PowerPoint、Outlook、OneNote、Access、Publisher を含む完全なオフィススイート
   - 最新の機能とセキュリティアップデートを提供

2. **Microsoft Office LTSC Standard 2024**
   - Word、Excel、PowerPoint、Outlook、OneNote を含む標準オフィススイート
   - 中小規模の組織に適した構成

3. **Microsoft Visio LTSC Professional 2024 / Standard 2024**
   - ダイアグラム、フローチャート、組織図などの作成ツール
   - Professional 版にはより高度な図表作成機能を搭載

4. **Microsoft Project Professional 2024 / Standard 2024**
   - プロジェクト管理とスケジューリングツール
   - Professional 版にはリソース管理とポートフォリオ分析機能を搭載

### マネージドアプリケーション機能

- **既存ワークフローの活用**: 既存の Manage application ワークフローを使用してアプリケーションを管理
- **柔軟な構成**: 新規または既存の WorkSpaces インスタンスにアプリケーションを追加可能
- **データ保持**: ユーザーボリュームとルートボリュームを保持したままアプリケーションを管理

## 技術仕様

### 対応バンドルとストレージ要件

| アプリケーション | 必要ディスク容量 | 対応バンドル |
|------------------|------------------|--------------|
| Microsoft Office 2024 | 最大 25 GB | すべての WorkSpaces バンドル |
| Microsoft Visio 2024 | 最大 25 GB | すべての WorkSpaces バンドル |
| Microsoft Project 2024 | 最大 25 GB | すべての WorkSpaces バンドル |

### バージョン互換性の制約

- Microsoft Office、Visio、Project は同じエディション (Standard または Professional) とバージョン (2021 または 2024) に統一する必要があります
- Value、Graphics、GraphicsPro WorkSpaces バンドルでは、Microsoft Office/Visio/Project 2021 Standard/Professional はサポートされていません

## 設定方法

### 前提条件

1. Amazon WorkSpaces Personal または Core のインスタンスが存在すること
2. WorkSpaces インスタンスのステータスが `AVAILABLE` または `STOPPED` であること
3. 適切な IAM 権限を持っていること
4. 十分なディスク容量 (最大 25 GB) が利用可能であること

### 手順

#### ステップ 1: WorkSpaces コンソールへのアクセス

WorkSpaces コンソールにアクセスし、アプリケーションを追加する WorkSpaces インスタンスを選択します。

#### ステップ 2: Manage application の実行

1. WorkSpaces インスタンスを選択
2. **Actions** メニューから **Manage applications** を選択
3. 追加するアプリケーション (Microsoft Office 2024、Visio 2024、Project 2024) を選択
4. 設定を確認して適用

#### ステップ 3: インストールの確認

WorkSpaces インスタンスにログインし、アプリケーションが正しくインストールされていることを確認します。

## メリット

### ビジネス面

- **迅速な展開**: 既存のバンドルを変更せずに、最新のアプリケーションを迅速に展開可能
- **コスト効率**: 必要なアプリケーションのみを選択して課金されるため、無駄なコストを削減
- **一貫性の確保**: 組織全体で統一された生産性環境を提供

### 技術面

- **簡素化された管理**: Manage application ワークフローを使用した一元管理
- **データ保持**: ユーザーデータとルートボリュームを保持したままアプリケーションを更新
- **柔軟性**: 新規および既存の WorkSpaces インスタンスに対応

## デメリット・制約事項

### 制限事項

- Microsoft Office、Visio、Project は同じエディションとバージョンに統一する必要があります
- Value、Standard、Graphics、GraphicsPro WorkSpaces バンドルでは Microsoft Visual Studio 2022 はサポートされていません
- オプトインリージョン (例: アフリカ ケープタウン) では、ディレクトリレベルで WorkSpaces のインターネット接続を有効にする必要があります

### 考慮すべき点

- WorkSpaces の復元、再構築、または移行を行うと、Manage applications ワークフローを使用してインストールされたアプリケーションバンドルは削除されますが、ライセンスはアクティブなままで課金が継続されます
- WorkSpaces の再構築では、インストールされたアプリケーションバンドルが削除および非アクティブ化され、削除されたアプリケーションバンドルがインストールおよびアクティブ化されます
- アプリケーションのインストールには十分なディスク容量 (最大 25 GB) が必要です

## ユースケース

### ユースケース 1: 組織全体への Office 2024 展開

**シナリオ**: 企業が既存の WorkSpaces 環境を持ち、組織全体で Microsoft Office 2024 に移行したい。

**実装例**:
既存の WorkSpaces インスタンスに対して、Manage application ワークフローを使用して Microsoft Office 2024 を追加します。ユーザーデータを保持したまま、最新バージョンへの移行が可能です。

**効果**:
- 移行時間の短縮
- ユーザーデータの保持
- 一貫した生産性環境の提供

### ユースケース 2: プロジェクトマネージャーへの Project 2024 提供

**シナリオ**: 特定の部門のプロジェクトマネージャーにのみ Microsoft Project 2024 を提供したい。

**実装例**:
該当する WorkSpaces インスタンスにのみ Microsoft Project Professional 2024 を追加し、必要なユーザーにのみライセンスコストを適用します。

**効果**:
- 必要なユーザーにのみアプリケーションを提供
- コストの最適化
- 柔軟なライセンス管理

### ユースケース 3: 設計チームへの Visio 2024 提供

**シナリオ**: 設計チームや IT チームに Microsoft Visio 2024 を提供し、図表作成機能を強化したい。

**実装例**:
設計チームの WorkSpaces インスタンスに Microsoft Visio Professional 2024 を追加し、高度な図表作成機能を提供します。

**効果**:
- 専門的な図表作成ツールの提供
- チーム間のコラボレーション向上
- 標準化されたドキュメント作成

## 料金

選択したアプリケーションに対して課金されます。料金は WorkSpaces インスタンスの料金に加えて、各アプリケーションの月額ライセンス料金が適用されます。

詳細な料金については、[Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/desktop-as-a-service/pricing/) を参照してください。

## 利用可能リージョン

この機能は、Amazon WorkSpaces Personal および Core をサポートするすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon WorkSpaces Personal**: 個人向けのフルマネージド仮想デスクトップサービス
- **Amazon WorkSpaces Core**: エンタープライズ向けのカスタマイズ可能な仮想デスクトップサービス
- **Amazon WorkSpaces Application Manager (WAM)**: アプリケーションのパッケージ化と配信を管理するサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/microsoft-office-visio-project-2024-amazon-workspaces/)
- [Manage applications ドキュメント](https://docs.aws.amazon.com/workspaces/latest/adminguide/manage-applications.html)
- [Administer your WorkSpaces ドキュメント](https://docs.aws.amazon.com/workspaces/latest/adminguide/administer-workspaces.html)
- [料金ページ](https://aws.amazon.com/workspaces/desktop-as-a-service/pricing/)

## まとめ

Amazon WorkSpaces が Microsoft Office 2024、Visio 2024、Project 2024 に対応したことで、組織は既存のバンドルを変更することなく、最新の生産性アプリケーションを迅速に展開できるようになりました。Manage application ワークフローを使用することで、ユーザーデータを保持したまま、柔軟にアプリケーションを管理できます。既存の WorkSpaces 環境を持つ組織は、この機能を活用して最新の生産性環境への移行を検討することをお勧めします。
