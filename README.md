# Amplify

AWS Amplifyを使用してシンプルなウェブアプリケーションを作成する。

最初のモジュールではAWSでReactアプリケーションを構築してホストする。

次のモジュールでは、CLIを使用してローカルアプリを初期化し、認証を追加し、GraphQL APIとデータベースを追加して、アプリを更新して画像を保存する。



## モジュール1. Reactアプリケーションをデプロイする

AWS Amplifyは、サーバーレスバックエンドを持つ静的ウェブサイト、単一ページのウェブアプリケーションの構築、デプロイ、ホスティングのためのワークフローを提供する。

Gitリポジトリに接続すると、AmplifyではAmplify CLIで設定されたフロントエンドフレームワークと任意のサーバーレスバックエンドのリソースの両方のビルド設定を決定し、全てのコードコミットを使用して自動的に更新をデプロイする。

まずは、Reactアプリケーションを作成し、それをGithubリポジトリにプッシュする。

その後、リポジトリをAWS Amplifyのウェブホスティングに接続し、amplifyapp.comドメイン上でホストされる、グローバルに利用可能なCDNにデプロイする。

次に、Reactアプリケーションに変化を加えることで継続的デプロイ機能を見ていく。



### 新しいReactアプリケーションの作成

次のようにcreate-react-appでパッケージのインストールを行う

```bash
$ npx create-react-app test-amplifyapp
$ cd amplifyapp
$ npm start
```



### Githubリポジトリの初期化

次のようにGithubリポジトリにアプリケーションをpushする

```bash
$ git init
$ git remote add origin ...
$ git add .
$ git commit -m 'initial commit'
$ git push origin master
```

