---
lab:
  title: "ラボ: 外部ソース管理を Azure Pipelines と統合する"
  module: "モジュール 6: Azure Pipelines を使用した継続的インテグレーションの実装"
---

# ラボ: 外部ソース管理を Azure Pipelines と統合する

# 学生用ラボ マニュアル

## ラボの概要

Azure DevOps の導入により、Microsoft は開発者に Azure Pipelines と呼ばれる新しい継続的インテグレーション/継続的デリバリー (CI/CD) サービスを提供します。これにより、任意のプラットフォームまたはクラウドに継続的にビルド、テスト、デプロイできます。Linux、macOS、Windows 向けのクラウドホスト型エージェントがあり、ネイティブ コンテナーをサポートし、Kubernetes、VM、サーバーレス環境に柔軟にデプロイできます。

Azure Pipelines は、無制限の CI/CD 分と 10 の並列ジョブをすべての Git Hub オープン ソース プロジェクトに無料で提供します。すべてのオープン ソース プロジェクトは、有料の顧客が使用するのと同じインフラストラクチャで実行されます。つまり、同じ高速パフォーマンスと高品質のサービスが得られます。Atom、CPython、Pipenv、Tox、Visual Studio Code、TypeScript など、トップのオープン ソース プロジェクトの多くはすでに CI/CD 用の Azure Pipelines を使用しており、そのリストは日々増え続けています。

このラボでは、GitHub プロジェクトを使用して Azure Pipelines を簡単にセットアップできることと、メリットをすぐに確認できることを確認します。

## 目標

このラボを完了すると、次のことができるようになります。

- GitHub Marketplace から Azure Pipelines をインストールします。
- GitHub プロジェクトを Azure DevOps パイプラインと統合します。
- パイプラインを介して Pull request を追跡します。

## ラボの所要時間 

- 推定時間: **60 分**

## 手順

### 開始する前に

#### ラボの仮想マシンにログインする

次の認証情報を使用して、Windows 10 仮想マシンにサインインしていることを確認します。

- ユーザー名: **Student**
- パスワード: **Pa55w.rd**

#### このラボに必要なアプリケーションを確認する

このラボで使用するアプリケーションを特定する:

- Microsoft Edge

#### Azure DevOps 組織を設定する

このラボで使用できる Azure DevOps 組織がまだない場合は、[組織またはプロジェクト コレクションの作成](https://docs.microsoft.com/ja-jp/azure/devops/organizations/accounts/create-organization?view=azure-devops)の手順に従って作成してください。

#### GitHub アカウントを設定する

このラボで使用できる GitHub アカウントをまだお持ちでない場合は、[新しい GitHub アカウントのサインアップ](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/signing-up-for-a-new-github-account)にある手順に従ってください。

### 演習 1: Azure Pipelines の使用を開始する

この演習では、Marketplace の新しい Azure Pipelines 統合を使用して、GitHub プロジェクトを Azure DevOps と統合します。

#### タスク 1: GitHub リポジトリをフォークして Azure Pipelines をインストールする

このタスクでは、GitHub リポジトリをフォークし、GitHub アカウントに AzurePipelines をインストールします。

1.  GitHub にまだサインインしていない場合は、ラボ コンピューターで、Web ブラウザーを起動し、[GitHub actionsdemos/calculator](https://github.com/actionsdemos/calculator)サイトに移動し、GitHub にサインインします。

    > **注**: これは、このラボでフォークして使用するベースライン プロジェクトです。

1.  **actionsdemos/calculator** ページで、「**Fork**」 をクリックして、リポジトリを自分の GitHub アカウントにフォークします。プロンプトが表示されたら、リポジトリをフォークするアカウントを選択します。

    > **注**: **GitHub Marketplace** には、プロジェクト ワークフローの拡張に役立つ、Microsoft およびサードパーティのさまざまなツールが用意されています。

1.  フォークされたリポジトリを表示しているページの上にあるメニューで、「**Marketplace**」 をクリックします。
1.  「**Search for apps and actions**」 に「**Azure Pipelines**」と入力し、**Enter** キーを押して、結果のリストで 「**Azure Pipelines**」 をクリックします。
1.  「**Azure Pipelines**」 ページで、中ほどにある「**Read more**」 をクリックし、Azure Pipelines の利点を確認します。

    > **注**: Azure Pipelines オファリングは、パブリック リポジトリに誰でも無料で使用でき、プライベート リポジトリを使用している場合は単一のビルド キューに無料で使用できます。

1.  「**Azure Pipelines**」 ページで、「**Install it for free**」 をクリックします。複数の **GitHub** アカウントがある場合は、「**Switch billing account**」 ドロップダウンから calculator をフォークしたアカウントを選択します。
1.  「**Review your order**」 ページで、「**Complete order and begin installation**」 をクリックします。
1.  「**Install Azure Pipelines**」 ページで、デフォルト オプション 「**All repositories**」 を使用して、「**Install**」 をクリックします。

    > **注**: 含めるリポジトリを指定するオプションがありますが、このラボでは、すべてのリポジトリを含めるだけです。Azure DevOps は、そのサービスを実行するために、リストされた一連のアクセス許可を必要とすることに注意してください。

1.  プロンプトが表示されたら、Azure DevOps のアカウントで認証して続行します。
1.  プロンプトが表示されたら、「**Setup your Azure Pipelines project**」 ページの 「**Select your Azure DevOps organization**」 ドロップダウン リストで、Azure DevOps アカウントを選択し、「**Create a new project**」 をクリックします。
1.  プロンプトが表示されたら、「**Setup your Azure Pipelines project**」 ページの 「**Project name**」 テキストボックスに「**Integrating External Source Control with Azure Pipelines**」と入力し、**Project visibility**を**Private**に設定したままにして、「**Continue**」 をクリックします。
1.  「**Azure Pipelines by Microsoft would like permission to**」 ページで、「**Authorize Azure Pipelines**」 をクリックします。

### タスク 2: Azure Pipelines プロジェクトを構成する

このタスクでは、前のタスクで作成した GitHub リポジトリのフォークに基づいて Azure Pipelines プロジェクトを構成します。

> **注**: これで Azure DevOps サイトにアクセスし、Azure Pipelines プロジェクトをセットアップする必要があります。

1.  Azure DevOps ポータルの 「**Pipelines**」 をクリックします。

1.  **Create pipeline**をクリックします。

1.  「**Select a repository**」 ペインで、「**GitHub**」を選択し、前のタスクで作成した、フォークされた **calculator** リポジトリを選択します。

    > **注**: Azure Pipelines は、既存のテンプレートが適切かどうかを判断するためにプロジェクトを分析します。この場合、推奨されるテンプレートは **Node.js** 用であり、これは私たちのニーズに最適です。いくつかの代替テンプレートも提案されていますが、このラボには推奨されるテンプレートが最適です。

1.  「**Configure your pipeline**」 で、「**Node.js**」 を選択します。

    > **注**: ビルド パイプラインは **YAML** として定義されます。これは、リポジトリ内の他のファイルと同じようにパイプラインの構成を管理できるため、このようなプロセスの定義に適したマークアップ構文です。これは、ビルド用に VM をプルするプール、ビルド用に Node.js をインストールするプロセス、および実際のビルド自体を識別する非常に単純なテンプレートです。

1.  「**Review your pipeline YAML**」で、「**Save and run**」 をクリックしてパイプラインを保存し、新しいビルドをキューに入れます。
1.  「**Save and run**」 ペインで、デフォルト設定を受け入れ、「**Save and run**」 をクリックします。

    > **注**: このラボでは、この新しいファイルをマスター ブランチに直接コミットできます。

    > **注**: パイプラインが完了するまで少し時間がかかります。この間、ビルド エージェントを構成し、GitHub からソースをプルして、パイプライン定義に従ってビルドします。

1.  ビルド ジョブのペインの 「**Summary**」 タブで、ビルドが正常に完了したことを確認します。

### タスク 3: YAML ビルド パイプライン定義を変更する

このタスクでは、フォークされた GitHub リポジトリの YAML ビルド定義を変更し、変更によってトリガーされたビルド プロセスを追跡します。

> **注**: デフォルトのパイプラインは素晴らしいスタートですが、自動化したいすべてのことを実行できるわけではありません。たとえば、テストを実行して、変更によってバグが発生しないことを確認できれば素晴らしいと思います。YAML を手動で編集できる GitHub に戻りましょう。

1.  ビルド ジョブのペインの 「**Summary**」 タブで、「**Repository and version**」 ラベルのそばにある、このラボで前に作成したフォークをホストしている **caluclator** リポジトリを右クリックし、「**Open link in new tab(リンクを新しいタブで開く)**」 を選択します。これにより、新しいブラウザー タブが開き、calculator コンテンツを含む GitHub ページが表示されます。

    > **注**: このラボでは、GitHub と Azure DevOps の間を行ったり来たりする必要があるため、それぞれに対してブラウザー タブを開いたままにしておく方が簡単です。

1.  フォークのコンテンツを表示している GitHub ページで、ファイル **azure-pipelines.yml** を表すエントリを見つけてクリックします。これにより、ファイルが自動的に開き、その内容が表示されます。
1.  **master/calculator/azure-pipelines.yml** ページのファイル コンテンツを表示しているペインの右上隅にある、鉛筆の形をした 「**Edit this file**」 アイコンをクリックします。

    > **注**: 私たちのプロジェクトにはすでに Mocha を使用して作成されたテストが含まれているため、パイプライン外で実行する必要があります。

1.  テスト実行を追加するには、同じインデントを使用して、`npm run build` コマンドのすぐ下に `npm test` コマンドを追加します。さらに、`display name` エントリを `'npm install, build, and test'` に更新して、ビルドの各タスクが実行していることを明確に示します。

    ```
    - script: |
        npm install
        npm run build
        npm test
      displayName: 'npm install, build, and test'
    ```

1.  ページの一番下までスクロールし、デフォルトのコミット メッセージを **Adding npm test** に置き換えて、「**Commit changes**」 をクリックします。

    > **注**: これもラボ環境であることを考慮して、この変更をマスター ブランチに直接コミットすることは許容されます。

1.  **Azure DevOps** ポータルを表示しているブラウザー タブに戻り、「**Pipelines**」 ビューの 「**Pipelines**」 ペインに移動します。
1.  更新によってトリガーされた新しいビルドが、「**Recently run pipelines**」 リストの 「**Recent**」 タブに既に表示されていることを確認します。エントリをクリックし、「**Runs**」 タブで最新の実行を選択し、「**Jobs**」 セクションで 「**Job**」 エントリをクリックします。
1.  ジョブの詳細を表示しているペインで、ジョブの個々のタスクをクリックして、完了まで実行します。

### タスク 4: GitHub Pull request を介して変更を提案する

このタスクでは、あえてエラーになる変更を行い、Pull request によってトリガーされたビルドの結果を確認します。

> **注**: このパイプライン設定の大きな利点の 1 つは、誰かが変更をコミットするたびに自動的に実行される品質ゲートがあることです。これにより、プロジェクトの管理がはるかに簡単になり、さまざまなレベルの品質が向上する可能性があります。

1.  **azure-pipelines.yml** ファイルのコンテンツを表示する GitHub ページに戻り、フォークされたリポジトリのコンテンツを一覧表示しているページに戻り、「**Go to file**」 をクリックします。
1.  **calculator/** プロンプトで、**arithmeticController.js** と入力し、結果のリストで **api/controllers/arithmeticController.js** をクリックします。これにより、ブラウザー セッションが自動的に **master/calculator/api/controllers/arithmeticController.js** ページにリダイレクトされ、そのファイルのコンテンツが表示されます。

    > **注**: このコントローラー（ソースコード）には、アプリの中核となる機能が含まれています。ただし、**add**操作のコードは完全には明確ではありません。やる気はあるが、Java Script の経験が不足している人の立場に立って考えてください。彼らはこれを、コードをクリーンアップして改善し、貢献する機会としてとらえるかもしれません。

1.  **master/calculator/api/controllers/arithmeticController.js** ページで、ファイルの内容を表示しているペインの右上隅にある、鉛筆の形をした 「**Edit this file**」 アイコンをクリックします。
1.  行 ` 'add': function(a,b) { return +a + +b },` を ` 'add': function(a,b) { return a + b },` に変更します。

    > **注**: これは誤った変更であり、エラーが発生します。

1.  ページの一番下までスクロールし、デフォルトのコミット メッセージを「**Modifying the add function**」に置き換え、「**Create a new branch**」 を選択し、その名前を「**addition-cleanup**」に設定して、「**Propose file change**」 をクリックします。
1.  「**Open a pull request**」 ページで、「**Create pull request**」 をクリックすると、テストされていない変更を本番コードに取り込むプロセスが自動的に開始します。**All chacks have failed** が表示されるまで待ってください。

    > **注**: Azure DevOps は 変更を検出し、ビルド パイプラインを開始します。これにより、チェックが失敗し、GitHub UI の更新がトリガーされます。

    > **注**: 「プロジェクト所有者」の立場に戻ってください。

1.  「**Modifying the add function #1**」 Pull request ページの 「**All checks have failed**」 セクションで、「**Details**」 をクリックして詳細を確認します。
1.  画面の一番下にある、「**View more details on Azure Pipelines**」 リンクをクリックします。これにより、新しいブラウザー タブが開き、Azure DevOps ポータルで失敗したジョブの実行が表示されます。
1.  Azure DevOps portal の失敗したジョブ ペインで、「**Job**」 エントリをクリックして詳細を表示します。
1.  ジョブ タスクのリストで、**npm install、build and test** タスクをクリックして、その出力を表示します。
1.  失敗したテストをリストするセクションを見つけます。

    > **注**: テストが失敗した理由はすぐにはわからない場合がありますが、パイプラインで蓄積されたすべての履歴により、この新しい Pull request からの何かが原因であることが簡単に特定できます。次のステップは、「21 + 21」が予想される「42」ではなく「2121」を生成した理由を理解することです。

1.  Azure DevOp s ポータルで失敗したジョブの実行を表示しているタブを閉じます。

### タスク 5: 壊れた Pull request を使用してプロジェクトを改善する

このタスクでは、前のタスクで作成された Pull request で導入された無効な変更を修正します。

> **注**: 「プロジェクト所有者」の立場に戻ってください。

1.  **Modifying the add function #1** GitHub ページを表示しているブラウザー タブに戻り、左上の **calculator** をクリックし、フォークされたリポジトリのコンテンツを一覧表示するメイン ページに戻ります。
1.  リポジトリ ファイルのリストを含むペインの上部で、**Pull requests**をクリックして開き、最新の **Pull request** を表すエントリ(**Modifying the add functions**)をクリックします。
1.  **Modifying the add function #1** GitHub ページで、「**Files changed**」 タブをクリックし、その内容を確認します。

    > **注**: 変更は、各変数の前のプラス記号がそれらの変数をそれらの数値表現に強制変換するために必要であることに気付いていない誰かによって行われたようです。それらを削除することにより、Java Script は中央のプラス記号を文字列連結演算子として解釈しました。これは、失敗したテストで 21 + 21 = 2121 である理由を説明しています。

1.  「**Modifying the add function #1**」 GitHub ページで、「**Review changes**」 ボタンのすぐ下にある省略記号をクリックし、ドロップダウン メニューで 「**Edit file**」 をクリックします。
1.  **a** 変数と **b** 変数の前にプラス記号を追加して元の変更を元に戻し、 `'add': function(a,b) { return +a + +b },` にします。さらに、前の行に「`// Using + operator to type cast variables as integers in order to prevent string concatenation`」というコメントを含めます。
1.  ページの一番下までスクロールし、デフォルトのコミット メッセージを「**Fixing the add function**」に置き換え、「**Commit directly to the addition-cleanup branch**」 オプションが選択されていることを確認して、「**Commit changes**」 をクリックします。
1.  **Modifying the add function #1** GitHub ページで、「**Conversation**」 タブを選択します。

    > **注**: Azure DevOps は変更を再度検出し、ビルド パイプラインを開始します。**All checks have passed** が表示されるまで待ちます。

1.  すべてのチェックに合格したら、「**Merge pull request**」 をクリックし、「**Confirm merge**」 をクリックします。

### タスク 6: ビルド ステータス バッジを追加する

このタスクでは、ビルド ステータス バッジを GitHub リポジトリに追加します。

> **注**: 高品質プロジェクトの重要な兆候は、ビルド ステータス バッジです。プロジェクトが現在正常なビルド状態にあることを示すバッジが付いているプロジェクトを誰かが見つけた場合、それはプロジェクトが効果的に維持されていることを示しています。

1.  **Azure DevOps** ポータルを表示しているブラウザー タブに戻り、**Pipelines** ビューの 「**Recent**」 タブに移動します。
1.  「**Recently run pipelines**」 リストの 「**Recent**」 タブで、このラボで使用したパイプラインに対応するエントリをクリックします。
1.  パイプライン ペインで、右上隅の省略記号をクリックし、ドロップダウンリスト で 「**Status badge**」 を選択します。

    > **注**: **Status badge** UI を使用すると、ビルド ステータスをすばやく簡単に参照できます。多くの場合、提供された URL を独自のダッシュボードで使用するか、Markdown スニペットを使用して Wiki ページなどの場所にステータス バッジを追加できます。

1.  「**Status badge**」 ペインで、「**Sample Markdown**」 の 「**Copy to clipboard**」 ボタンをクリックします。
1.  **caluclator** リポジトリの GitHub ページに戻り、画面の左上にある 「**<> Code**」 タブをクリックします。
1.  リポジトリ ファイルのリストで、「**README.md**」 をクリックし、**master/calculator/README.md** ページで、ファイルの内容を表示しているペインの右上隅にある 「**Edit this file**」 アイコンをクリックします。
1.  6 行目の上に行を追加し、クリップボードの内容を貼り付けます。
1.  ページの一番下までスクロールし、デフォルトのコミット メッセージを「**Add an Azure Pipelines status badge**」 に置き換えて、「**Commit changes**」 をクリックします。

    > **注**: で、プロジェクトのフロント ページに動的なビルド ステータス バッジが表示され、プロジェクトを効果的に管理していることを全員に知らせることができます。

#### レビュー

このラボでは、Marketplace の新しい Azure Pipelines 統合を使用して、GitHub プロジェクトを Azure DevOps と統合しました。
