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



### AWSマネジメントコンソールへのログイン

サービス名で[AWS Amplifyを選択する]

![AWSマネジメントコンソール](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20AWS%20Console%20Find%20Amplify%20Module%201.47a33baea2346b85a1d4bd157b9df5bebe693c4b.png)



### アプリをAWS Amplifyでデプロイする

amplifyのメニューで[Deliver]を選択する

次のようにリポジトリサービスを選択する

![リポジトリサービス](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20Amplify%20GitHub%20Module%201.04ba11118824e16e3d7d418d4f211d75a4649d1e.png)

さらに、Githubの認証を行い、作成したリポジトリとブランチを選択する。

ビルド設定を選択する画面では、デフォルトの設定を使用する。

最後に、[保存してデプロイ]を選択する。

次の画面になるので、左下のリンクからウェブアプリへアクセスする。

![デプロイ後amplify画面](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20Amplify%20Deploy%20Source%20Module%201.00becc211a8ecd42349cffb87406449074ed2e5c.png)



## ローカルAmplifyアプリを初期化する

アカウントで新しいAmplifyプロジェクトを初期化したので、開発を続行して新しい機能を追加できるようにプロジェクトをローカル環境に持ち込む。

そのために、Amplify CLIをインストールし、CLIを使用してAmplifyプロジェクトを初期化する。



### Amplify CLIをインストールする

Amplify CLIはシンプルなガイド付きワークフローに従って、アプリのAWSクラウドサービスを作成するための統合ツールチェーン。

```bash
$ npm install -g @aws-amplify/cli
```



### Amplify CLIの設定

IAMを使用すると、AWSでユーザーとユーザーアクセス許可を管理できる。

```bash
$ amplify configure
```

この際、terminalの指示に従ってiamユーザーを発行する必要がある。



### Amplifyアプリを初期化する

1. Amplifyコンソールで[バックエンド環境]をクリックする

2. [バックエンド環境]タブで、amplify initコマンドをキーボードにコピーする

3. 次のコマンドで、Amplifyプロジェクトをローカルで初期化する

    ```bash
    $ amplify init --appId ...
    ? Enter a name for the project: amplifyapp
    ? Enter a name for the environment: dev
    ? Choose your default editor: Visual Studio Code
    ? Choose the type of app that youre building: javascript
    ? What javascript framework are you using: react
    ? Source Directory Path: src
    ? Distribution Directory Path: build
    ? Build Command:  npm run-script build
    ? Start Command: npm run-script start
    ? Do you want to use an AWS profile? Y
    ? Please choose the profile you want to use: your-aws-profile
    ```

    

## 認証の追加

マネージド型のユーザーIDサービスであるAmazon Cognitoを利用して、Amplify CLIとライブラリでユーザーを認証する方法について

Amplify UIコンポーネントライブラリを使用してユーザー認証フロー全体を構築する方法を学び、ユーザーが数行のコードでサインアップ、サインイン、パスワードのリセットを行えるようにする



### Amplifyライブラリをインストールする

プロジェクトには2つのAmplifyライブラリが必要。

メインのaws-amplifyライブラリには、使用するさまざまなawsのサービスとやりとりするための全てのクライアント側APIが含まれ、@aws-amplify/ui-reactライブラリにはフレームワーク固有のUIコンポーネントが含まれる。

```bash
$ npm install aws-amplify @aws-amplify/ui-react
```



### 認証サービスを作成する

認証サービスを作成するには、Amplify CLIを使用する

```bash
$ amplify add auth

? Do you want to use the default authentication and security configuration? Default configuration
? How do you want users to be able to sign in? Username
? Do you want to configure advanced settings? No, I am done.
```



### 認証サービスをデプロイする

認証サービスがローカルで設定できたので、Amplify pushコマンドを使用して認証サービスをデプロイできる

```bash
$ amplify push --y
```



### Amplifyリソースを使用してReactプロジェクトを設定する

プロジェクトのsrcディレクトリにあるaws-exports.jsというファイルにより、Amplifyプロジェクトで使用できる様々なAWSリソースについてReactプロジェクトで使用できるようになる。

src/index.jsを開き、importの下に次のコードを追加する。

```jsx
import Amplify from 'aws-amplify'
import config from './aws-exports'
Amplify.configure(config)
```



### App.jsに認証フローを追加する

次に、src/App.jsを開き、次のように更新する

```jsx
import React from 'react'
import logo from './logo.svg'
import './App.css'
import { withAuthenticator, AmplifySignOut } from '@aws-amplify/ui-react'

function App() {
    return (
    	<div className="App">
        	<header>
            	<img src={logo} className="App-logo" alt="logo" />
                <h1>We now have Auth!</h1>
            </header>
            <AmplifySignOut />
        </div>
    )
}

export default withAuthenticator(App)
```

このコンポーネントは、ユーザー認証フロー全体を足場にして、ユーザーがサインアップ、サインイン、パスワードのリセット、MFAのサインインの確認を行えるようにする。

サインアウトボタンのレンダリングのためのAmplifySignOutコンポーネントも使用している。



### アプリをローカルで実行する

アプリを実行して、新しい認証フローを確認する。

```bash
$ npm start
```

次のようにサインイン用の画面が確認できる

![cognitoを使用したreactサインイン](https://d1.awsstatic.com/webteam/getting_started/GSRC%202020%20updates/Front%20End/Front%20End%20Sign%20In%20Screen%20Module%203.dc66e4d98879ec9b68577be8b0909683141f187e.png)



### ライブ環境へのデプロイ

最後に、githubにデプロイしてAmplifyコンソールで新しいビルドを開始する。

この際、指示通りにやるとエラーが生じるので、このサイトを参考に何点か修正した。

https://miruohotspring.net/blog/aws-exports-build-option/

https://docs.aws.amazon.com/ja_jp/amplify/latest/userguide/how-to-service-role-amplify-console.html



## GraphQL APIとデータベースを追加する

上記の手順でアプリの作成と認証を設定できたので、次はAPIを追加する。

このモジュールでは、Amplify CLIとライブラリを使用してアプリにAPIを追加する。

作成するAPIはGraphQL APIで、Amazon DynamoDB(NoSQL)によってサポートされるAWS AppSyncを利用する。

作成するアプリは、ユーザーがメモを作成・削除・一覧表示できるようにするメモアプリ。



### GraphQL APIとデータベースを作成する

アプリディレクトリのルートから次のコマンドを実行し、GraphQL APIをアプリに追加する

```bash
$ amplify add api
? Please select from one of the below mentioned services: GraphQL
? Provide API name: notesapp
? Choose the default authorization type for the API: API Key
? Enter a description for the API key: demo
? After how many days from now the API key should expire: 7 (or your preferred expiration)
? Do you want to configure advanced settings for the GraphQL API: No, I am done.
? Do you have an annotated GraphQL schema?  No
? Do you want a guided schema creation?  Yes
? What best describes your project: Single object with fields
? Do you want to edit the schema now? Yes
```



