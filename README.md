# 本レポジトリの目的

next.jsをAzure App Serviceで動かすための方法をまとめる。
なお、本レポジトリでは小規模運用を想定しており、DockerやKubernetesといったコンテナ活用をスコープとしていない。
別途レポジトリを整備する予定。

# 開発サーバー

プロジェクトルートレベルで下記を実行する。

```
npm run dev
```

# インフラの初期化


ルートディレクトリにARMテンプレートであるazuredeploy.jsonを利用してAzureリソースを初期化できる。

ARMテンプレートは、JSON形式のファイルで、リソースグループ、App Serviceプラン、WebアプリなどのAzureリソースを定義。
本ARMテンプレートでは東日本リージョンにBasicレベルでLinuxのNode.jsの18のリソースを作成する。

以下は、このARMテンプレートの各セクションの簡単な説明。

`$schema`：テンプレートのスキーマへのリンク。これは、テンプレート検証のために使用される。

`contentVersion`：テンプレートのバージョン。これは開発者が追跡や管理を容易にするために使用される。

`parameters`：テンプレートで使用される入力パラメータを定義します。このテンプレートでは、`resourceGroupName`、`appServicePlanName`、および`webAppName`の3つのパラメータが定義されている。

`resources`：デプロイされるAzureリソースを定義するセクション。このテンプレートでは、2つのリソースが定義されている。

App Serviceプラン（Microsoft.Web/serverfarms）: B1 SKU（Basic）を持つLinuxベースのApp Serviceプランを作成する。 

App Serviceプランは、Webアプリのリソースを確保するために使用されます。

Webアプリ（Microsoft.Web/sites）: Node.jsランタイムを使用して実行されるWebアプリを作成する。  

Webアプリは、先に作成されたApp Serviceプランに依存する。（dependsOnセクション参照）。また、siteConfigプロパティを通じて、アプリ設定とランタイムバージョンが定義されています。
このARMテンプレートを使用すると、Azureリソースのデプロイが簡単に自動化できます。通常、このテンプレートはAzure DevOpsやGitHub ActionsなどのCI/CDパイプラインと組み合わせて使用される。