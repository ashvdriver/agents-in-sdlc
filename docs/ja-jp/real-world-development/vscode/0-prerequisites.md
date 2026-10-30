---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、すべての準備を整える必要があります。Tailspin Toys リポジトリの自分用のコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作していることを確認します。

## ラボ用リポジトリを設定する

作成するコードのためのリポジトリのコピーを、[テンプレート][template-repository]から作成します。新しいリポジトリにはラボに必要なすべてのファイルが含まれており、演習を進める際に使用します。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択して、自分用のリポジトリのコピーを作成します。次に **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップを受講している場合は、メンターから提供される指示に従ってください。それ以外の場合は、GitHub Copilot にアクセスできる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。ラボの後半で参照します。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップを通じてこれらの Issue に沿って作業するため、自分で登録する必要はありません。

## codespace を作成する

次に、codespace を使ってラボの演習を行います。

[GitHub Codespaces][codespaces] は、ブラウザー上で直接コードを記述、実行、デバッグできる、クラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、フル機能の IDE を提供します。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに高速です。この時間を使って、GitHub Copilot の他の機能を確認できます。次にそちらに注目します。

> [!CAUTION]
> 今後の演習で codespace に戻ります。当面は、ブラウザーのタブで開いたままにしておいてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの[開発コンテナー][dev-containers]内で実行するように作られています。どちらも、スムーズに進められるよう、必要な前提条件がすべてインストールされた環境を提供します。ローカルで実行する場合は、複製したリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。VS Code が、codespace と同じ開発コンテナーをビルドします。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使用する

GitHub Copilot Chat のエージェントモードを使用するには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使用している場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code で拡張機能ビューを開き、**GitHub Copilot Chat** を検索して、**Install** を選択します。

拡張機能をインストールしたら、有効にするために GitHub アカウントでの認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに "Hello world" のようなメッセージを入力し、Enter キーを押します。これにより、Copilot Chat が有効になります。
4. または、認証されていない場合は、GitHub アカウントへのサインインを求められます。指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

お疲れさまでした。ラボ用リポジトリのコピーを作成できました。また、コードを書き始めるときに使用する codespace の作成も開始しました。

## 次のステップ

Copilot を実際に活用していきましょう。[演習 1 - カスタム指示][next-lesson]に進み、プロジェクトの規約を Copilot に教えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
