---
title: アクションについて
intro: 'アクションは個々のタスクで、組み合わせてジョブを作成したりワークフローをカスタマイズしたりできます。 独自のアクションを作成したり、 {{ site.data.variables.product.prodname_dotcom }} コミュニティで共有するアクションを使用したりカスタマイズしたりできます。'
product: '{{ site.data.reusables.gated-features.actions }}'
redirect_from:
  - /articles/about-actions
  - /github/automating-your-workflow-with-github-actions/about-actions
  - /actions/automating-your-workflow-with-github-actions/about-actions
  - /actions/building-actions/about-actions
versions:
  free-pro-team: '*'
  enterprise-server: '>=2.22'
---

{{ site.data.reusables.actions.enterprise-beta }}
{{ site.data.variables.product.prodname_dotcom }}は、macOSランナーのホストに[MacStadium](https://www.macstadium.com/)を使用しています。

### アクションについて

{{ site.data.variables.product.prodname_dotcom }}の API やパブリックに利用可能なサードパーティAPIとのインテグレーションなど、好きな方法でリポジトリを操作するカスタムコードを書いて、アクションを作成することができます。 たとえば、アクションでnpmモジュールを公開する、緊急の問題が発生したときにSMSアラートを送信する、本番対応のコードをデプロイすることなどが可能です。

{% if currentVersion == "free-pro-team@latest" %}
ワークフローで使用する独自のアクションを作成したり、ビルドしたアクションを{{ site.data.variables.product.prodname_dotcom }}コミュニティと共有したりできます。 ビルドしたアクションをシェアするには、リポジトリをパブリックにする必要があります。
{% endif %}

アクションはマシン上で直接実行することも、Dockerコンテナで実行することもできます。 アクションの入力、出力、環境変数を定義できます。

### アクションの種類

DockerコンテナのアクションとJavaScriptのアクションをビルドできます。 アクションには、アクションの入力、出力、およびメインのエントリポイントを定義するメタデータファイルが必要です。 このメタデータのファイル名は`action.yml`もしくは`action.yaml`でなければなりません。 詳しい情報については、「[{{ site.data.variables.product.prodname_actions }} のメタデータ構文](/articles/metadata-syntax-for-github-actions)」を参照してください。

| 種類         | オペレーティングシステム        |
| ---------- | ------------------- |
| Dockerコンテナ | Linux               |
| JavaScript | Linux、MacOS、Windows |
| 複合実行ステップ   | Linux、MacOS、Windows |

#### Docker コンテナーアクション

Dockerコンテナは、{{ site.data.variables.product.prodname_actions }}コードで環境をパッケージ化します。 アクションの利用者がツールや依存関係を考慮しなくて済むため、作業単位の一貫性と信頼性が向上します。

Dockerコンテナを使用すると、Osのバージョン、依存関係、ツール、コードを限定することができます。 特定の環境設定で実行しなければならないアクションの場合、オペレーティングシステムとツールを選択できるので、Dockerは理想的な選択肢です。 コンテナのビルドおよび取得のレイテンシにより、DockerコンテナのアクションはJavaScriptアクションより遅くなります。

Docker コンテナアクションは、Linux オペレーティングシステムのランナーでのみ実行できます。 {{ site.data.reusables.github-actions.self-hosted-runner-reqs-docker }}

#### アクション

JavaScriptアクションはランナーマシン上で直接実行でき、アクションのコードはそのコードを実行するのに使われた環境から分離できます。 JavaScriptのアクションを使うと、アクションコードが単純になり、実行も Dockerコンテナのアクションより速くなります。

{{ site.data.reusables.github-actions.pure-javascript }}

Node.jsプロジェクトの開発では、{{ site.data.variables.product.prodname_actions }} Toolkitで提供するパッケージを使って、開発の速度を上げることができます。 詳しい情報については、[actions/toolkit](https://github.com/actions/toolkit) リポジトリ以下を参照してください。

#### 複合実行ステップアクション

_複合実行ステップ_ アクションを使用すると、複数のワークフロー実行ステップを 1 つのアクション内で結合できます。 たとえば、この機能を使用して複数の実行コマンドを 1 つのアクションにバンドルし、そのアクションを使用してバンドルされたコマンドを 1 つのステップで実行するワークフローを作成できます。 例を参照するには、「[複合実行ステップの作成アクション](/actions/creating-actions/creating-a-composite-run-steps-action)」を確認してください。

### アクションの場所を選択する

他のユーザーが使うアクションを開発する場合には、他のアプリケーションコードにバンドルするのではなく、アクションをそれ自体のリポジトリに保持しておくことをお勧めします。 こうすると、他のソフトウェアと同様にアクションのバージョニング、追跡、リリースが可能になるからです。

{% if currentVersion == "free-pro-team@latest" %}
アクションをそれ自体のリポジトリに保存すると、{{ site.data.variables.product.prodname_dotcom }}コミュニティがアクションを見つけやすくなります。また、開発者がアクションの問題を解決したり機能を拡張したりするとき、コードベースのスコープが限定され、アクションのバージョニングが他のアプリケーションコードのバージョニングから切り離されます。
{% endif %}

ビルドしているアクションをパブリックに公開する予定がない場合、アクションのファイルはリポジトリのどの場所に保存してもかまいません。 アクション、ワークフロー、アプリケーションコードを 1 つのリポジトリで組み合わせる予定の場合、アクションは `.github` ディレクトリに保存することをお勧めします。 たとえば、`.github/actions/action-a`や`.github/actions/action-b`に保存します。

### {{ site.data.variables.product.prodname_ghe_server }}との互換性

アクションが {{ site.data.variables.product.prodname_ghe_server }}と互換性があることを確認するには、 {{ site.data.variables.product.prodname_dotcom }} API URL へのハードコーディングされた参照を使用しないようにする必要があります。 代わりに、環境変数を使用して {{ site.data.variables.product.prodname_dotcom }} API を参照する必要があります。

- REST API の場合は、 `GITHUB_API_URL` 環境変数を使用します。
- GraphQL の場合は、 `GITHUB_GRAPHQL_URL` 環境変数を使用します。

詳細については、「既定の環境変数</a>

」を参照してください。</p> 



### アクションにリリース管理を使用する

このセクションでは、リリース管理を使用してアクションへの更新を予測可能な方法で配布する方法について説明します。



#### リリース管理の良い方法

他のユーザが使用するアクションを開発している場合は、リリース管理を使用して、更新の配布方法を管理することをお勧めします。 既存のワークフローとの互換性を維持しつつ、アクションのメジャーバージョンに必要な重要な修正およびセキュリティパッチも含まれます。 変更が互換性に影響する場合は、新しいメジャーバージョンのリリースを検討する必要があります。

このリリース管理アプローチでは、アクションが最新のコードを含む可能性が高く、結果として不安定になる可能性があるため、ユーザはアクションの `master` ブランチを参照しないでください。 代わりに、ユーザにアクションの使用時にメジャーバージョンを指定するように勧めて、問題が発生した場合にのみ、特定のバージョンを指定するようにすることができます。

特定のアクションのバージョンを使用するために、ユーザは {{ site.data.variables.product.prodname_actions }} ワークフローを設定して、タグ、コミットの SHA、またはリリースの名前が付けられたブランチをターゲットにすることができます。



#### タグを使用したリリース管理

アクションのリリース管理にはタグを使用することをお勧めします。 この方法を使用すると、ユーザはメジャーバージョンとマイナーバージョンを簡単に区別できます。

- リリースタグ（`v1.0.2` など）を作成する前に、リリースブランチ（`release/v1` など）でリリースを作成して検証します。
- セマンティックバージョニングを使用してリリースを作成します。 詳細は「[リリースを作成する](/articles/creating-releases)」を参照してください。
- 現在のリリースの Git ref を指すようにメジャーバージョンタグ（`v1`、`v2` など）を移動します。 詳細については、「[Gitの基本 - タグ](https://git-scm.com/book/en/v2/Git-Basics-Tagging)」を参照してください。
- 既存のワークフローを破壊する破壊的変更のための新しいメジャーバージョンタグ（`v2`）を導入します。 たとえば、アクションの入力の変更は破壊的変更です。
- メジャーバージョンは、最初のリリース時にそのステータスを示す `beta` タグ（`v2-beta` など）を付けることができます 。 その後、準備ができたら `-beta` タグを削除できます。

次の例は、ユーザがメジャーリリースタグを参照する方法を示しています。



```yaml
steps:
    - uses: actions/javascript-action@v1
```


次の例は、ユーザが特定のパッチリリースタグを参照する方法を示しています。



```yaml
steps:
    - uses: actions/javascript-action@v1.0.1
```




#### ブランチを使用したリリース管理

リリース管理にブランチ名を使用する場合、次の例では名前付きブランチを参照する方法を示しています。



```yaml
steps:
    - uses: actions/javascript-action@v1-beta
```




#### コミットの SHA を使用したリリース管理

各 Git コミットは、計算された SHA 値を受け取ります。これは一意で不変のものです。 アクションのユーザは、コミットの SHA 値に依存することを好む場合があります。削除や移動ができるタグを指定するよりこの方法のほうが信頼できるためです。 ただし、これは、ユーザがアクションに対して行われた更新をそれ以上受け取らないことを意味しています。 省略された値の代わりにコミットの完全な SHA 値を使用すると、同じ省略形を使用する悪意のあるコミットの使用を防ぐことができます。



```yaml
steps:
    - uses: actions/javascript-action@172239021f7ba04fe7327647b213799853a9eb89
```




### アクションのREADMEファイルを作成する 

アクションをパブリックに共有する予定がある場合には、アクションの使用方法を伝えるため README ファイルを作成することをお勧めします。 `README.md` には、以下の情報を含めることができます:

- アクションが実行する内容の説明
- 必須の入力引数と出力引数
- オプションの入力引数と出力引数
- アクションが使用するシークレット
- アクションが使用する環境変数
- ワークフローにおけるアクションの使用例



### {{ site.data.variables.product.prodname_github_apps}}に対する{{ site.data.variables.product.prodname_actions }}の比較

{{ site.data.variables.product.prodname_marketplace }}は、ワークフローを改善するツールを提供します。 それぞれのツールの違いや利点を理解すれば、自分の作業に最も適したツールを選択できるようになります。 アクションとアプリの構築の詳細については、「[の 「アプリの](/actions/getting-started-with-github-actions/about-github-actions)について 」および「アプリについて</a>」を参照してください。</p> 



#### GitHub ActionsとGitHub Appsの強み

{{ site.data.variables.product.prodname_actions }}と{{ site.data.variables.product.prodname_github_app }}はどちらもビルドの自動化の方法とワークフローツールを提供しますが、これらはそれぞれ異なる強みを持っており、違ったやり方で役立ちます。

{{ site.data.variables.product.prodname_github_apps }}は：

* 永続的に動作し、イベントに素早く反応できます。
* 永続化されたデータが必要な場合にうまく動作します。
* 時間のかからないAPIリクエストとうまく働きます。
* ユーザが提供するサーバーあるいはコンピューティングインフラストラクチャ上で動作します。

{{ site.data.variables.product.prodname_actions }}は：

* 継続的インテグレーションや継続的デプロイメントを実行する自動化を提供します。
* ランナーマシン上でもDockerコンテナ内でも直接実行できます。
* リポジトリのクローンへのアクセスを含めて、コードにアクセスするツール、コードフォーマッタ、コマンドラインツールをデプロイしたり公開したりできます。
* コードのデプロイやアプリケーションの提供が必要ありません。
* シークレットの生成と利用のためのシンプルなインターフェースを持っており、アクションを利用する人の認証情報を保存せずにサードパーティのサービスとアクションを連携できます。



### 参考リンク

- "[{{ site.data.variables.product.prodname_actions }}の開発ツール](/articles/development-tools-for-github-actions)"