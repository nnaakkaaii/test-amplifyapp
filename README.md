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



GraphQLスキーマ (amplify/backend/api/myapi/schema.graphql) を開き、次のように編集する

```
type Note @model {
	id: ID!
	name: String!
	description: String
}
```



### APIをデプロイする

APIがローカルで設定されたので、それをデプロイする。

このために、Amplify pushコマンドを実行する

```bash
$ amplify push --y
```

これにより、

1. AppSync APIを作成
2. DynamoDBテーブルを作成
3. APIのクエリに使用できるsrc/graphqlにあるフォルダにローカルGraphQL操作を行う



### APIと対話するためのフロントエンドコードを記述する

バックエンドがデプロイされたので、ユーザーがメモを作成・一覧表示・削除できるようにコードを記述する

```jsx
import React, { useState, useEffect } from 'react';
import './App.css';
import { API } from 'aws-amplify';
import { withAuthenticator, AmplifySignOut } from '@aws-amplify/ui-react';
import { listNotes } from './graphql/queries';
import { createNote as createNoteMutation, deleteNote as deleteNoteMutation } from './graphql/mutations';

const initialFormState = { name: '', description: '' }

function App() {
  const [notes, setNotes] = useState([]);
  const [formData, setFormData] = useState(initialFormState);

  useEffect(() => {
    fetchNotes();
  }, []);

  async function fetchNotes() {
    const apiData = await API.graphql({ query: listNotes });
    setNotes(apiData.data.listNotes.items);
  }

  async function createNote() {
    if (!formData.name || !formData.description) return;
    await API.graphql({ query: createNoteMutation, variables: { input: formData } });
    setNotes([ ...notes, formData ]);
    setFormData(initialFormState);
  }

  async function deleteNote({ id }) {
    const newNotesArray = notes.filter(note => note.id !== id);
    setNotes(newNotesArray);
    await API.graphql({ query: deleteNoteMutation, variables: { input: { id } }});
  }

  return (
    <div className="App">
      <h1>My Notes App</h1>
      <input
        onChange={e => setFormData({ ...formData, 'name': e.target.value})}
        placeholder="Note name"
        value={formData.name}
      />
      <input
        onChange={e => setFormData({ ...formData, 'description': e.target.value})}
        placeholder="Note description"
        value={formData.description}
      />
      <button onClick={createNote}>Create Note</button>
      <div style={{marginBottom: 30}}>
        {
          notes.map(note => (
            <div key={note.id || note.name}>
              <h2>{note.name}</h2>
              <p>{note.description}</p>
              <button onClick={() => deleteNote(note)}>Delete note</button>
            </div>
          ))
        }
      </div>
      <AmplifySignOut />
    </div>
  );
}

export default withAuthenticator(App);
```

アプリには3つの主要な機能がある

1. fetchNotes

    この関数は、APIクラスを使用してクエリをGraphQL APIに送信し、メモのリストを取得する

2. createNote

    この関数は、APIクラスを使用してmutationをGraphQL APIに送信する。

    主な違いは、この関数ではGraphQL mutationに必要な変数を渡して、フォームデータで新しいノートを作成できること。

3. deleteNote

    createNoteと同様に、この関数は変数と共にGraphQL mutationを送信するが、作成ではなく削除を行う



## ストレージを追加する

メモアプリが機能するようになったので、各メモに画像を関連付ける機能を追加する。

このモジュールでは、Amplify CLIとライブラリを使用して、Amazon S3を利用するストレージサービスを作成する。

次に、GraphQLスキーマを更新し、画像を各メモに関連付ける。

最後にReactアプリを更新して、画像のアップロード、フェッチ、レンダリングを有効にする



### ストレージサービスを作成する

画像ストレージ機能を追加するには、Amplifyストレージカテゴリを使用する

```bash
$ amplify add storage

? Please select from one of the below mentioned services: Content
? Please provide a friendly name for your resource that will be used to label this category in the project: imagestorage
? Please provide bucket name: <your-unique-bucket-name>
? Who should have access: Auth users only
? What kind of access do you want for Authenticated users? create, read, update, delete
? Do you want to add a Lambda Trigger for your S3 Bucket? N

```



### GraphQLスキーマを更新する

amplify/backend/api/notesapp/schema.graphqlを開き、次のように更新する

```
type Note @model {
  id: ID!
  name: String!
  description: String
  image: String
}
```



### ストレージサービスとAPIの更新をデプロイする

ストレージサービスがローカルで設定され、GraphQLスキーマが更新されたので、Amplify pushコマンドを実行して更新をデプロイする

```bash
$ amplify push --y
```



### Reactアプリを更新する

バックエンドが更新されたので、Reactアプリを更新して、メモ用に画像をアップロードおよび表示する機能を追加する。

1. ストレージクラスをAmplifyインポートに追加する

    ```jsx
    import { API, Storage } from 'aws-amplify';
    ```

2. メインのApp関数で、画像のアップロードを処理する新しいonChange関数を作成する

    ```jsx
    async function onChange(e) {
      if (!e.target.files[0]) return
      const file = e.target.files[0];
      setFormData({ ...formData, image: file.name });
      await Storage.put(file.name, file);
      fetchNotes();
    }
    ```

3. メモに関連付けられている画像がある場合は、fetchNotes関数を更新して画像を取得する

    ```jsx
    async function fetchNotes() {
      const apiData = await API.graphql({ query: listNotes });
      const notesFromAPI = apiData.data.listNotes.items;
      await Promise.all(notesFromAPI.map(async note => {
        if (note.image) {
          const image = await Storage.get(note.image);
          note.image = image;
        }
        return note;
      }))
      setNotes(apiData.data.listNotes.items);
    }
    ```

4. 画像がメモに関連付けられている場合は、createNote 関数を更新して、画像をローカル画像配列に追加します。

    ```jsx
    async function createNote() {
      if (!formData.name || !formData.description) return;
      await API.graphql({ query: createNoteMutation, variables: { input: formData } });
      if (formData.image) {
        const image = await Storage.get(formData.image);
        formData.image = image;
      }
      setNotes([ ...notes, formData ]);
      setFormData(initialFormState);
    }
    ```

5. returnブロックのフォームにその他の入力を追加する

    ```jsx
    <input
      type="file"
      onChange={onChange}
    />
    ```

6. メモ配列をマッピングするときに、画像が存在する場合はそれをレンダリングする

    ```jsx
    {
      notes.map(note => (
        <div key={note.id || note.name}>
          <h2>{note.name}</h2>
          <p>{note.description}</p>
          <button onClick={() => deleteNote(note)}>Delete note</button>
          {
            note.image && <img src={note.image} style={{width: 400}} />
          }
        </div>
      ))
    }
    ```





