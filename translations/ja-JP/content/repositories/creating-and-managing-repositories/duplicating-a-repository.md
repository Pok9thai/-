---
title: リポジトリを複製する
intro: 'To maintain a mirror of a repository without forking it, you can run a special clone command, then mirror-push to the new repository.'
redirect_from:
  - /articles/duplicating-a-repo/
  - /articles/duplicating-a-repository
  - /github/creating-cloning-and-archiving-repositories/duplicating-a-repository
  - /github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/duplicating-a-repository
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Repositories
---

{% ifversion fpt or ghec %}

{% note %}

**Note:** If you have a project hosted on another version control system, you can automatically import your project to {% data variables.product.prodname_dotcom %} using the {% data variables.product.prodname_dotcom %} Importer tool. For more information, see "[About {% data variables.product.prodname_dotcom %} Importer](/github/importing-your-projects-to-github/importing-source-code-to-github/about-github-importer)."

{% endnote %}

{% endif %}

Before you can push the original repository to your new copy, or _mirror_, of the repository, you must [create the new repository](/articles/creating-a-new-repository) on {% data variables.product.product_location %}. 以下の例では、`exampleuser/new-repository` および `exampleuser/mirrored` がミラーです。

## リポジトリをミラーする

{% data reusables.command_line.open_the_multi_os_terminal %}
2. リポジトリのベアクローンを作成します。
  ```shell
  $ git clone --bare https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>old-repository</em>.git
  ```
3. 新しいリポジトリをミラープッシュします。
  ```shell
  $ cd <em>old-repository</em>
  $ git push --mirror https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>new-repository</em>.git
  ```
4. 先ほど作成した一時ローカルリポジトリを削除します。
  ```shell
  $ cd ..
  $ rm -rf <em>old-repository</em>
  ```

## {% data variables.large_files.product_name_long %} オブジェクトを含むリポジトリをミラーする

{% data reusables.command_line.open_the_multi_os_terminal %}
2. リポジトリのベアクローンを作成します。 ユーザ名の例をリポジトリを所有する人や Organization の名前に置き換え、リポジトリ名の例を複製したいリポジトリの名前に置き換えてください。
  ```shell
  $ git clone --bare https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>old-repository</em>.git
  ```
3. クローンしたリポジトリに移動します。
  ```shell
  $ cd <em>old-repository</em>
  ```
4. リポジトリの {% data variables.large_files.product_name_long %} オブジェクトをプルします。
  ```shell
  $ git lfs fetch --all
  ```
5. 新しいリポジトリをミラープッシュします。
  ```shell
  $ git push --mirror https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>new-repository</em>.git
  ```
6. リポジトリの {% data variables.large_files.product_name_long %} オブジェクトをミラーにプッシュします。
  ```shell
  $ git lfs push --all https://github.com/<em>exampleuser/new-repository.git</em>
  ```
7. 先ほど作成した一時ローカルリポジトリを削除します。
  ```shell
  $ cd ..
  $ rm -rf <em>old-repository</em>
  ```

## 別の場所にあるリポジトリをミラーする

元のリポジトリから更新を取得するなど、別の場所にあるリポジトリをミラーする場合は、ミラーをクローンして定期的に変更をプッシュできます。

{% data reusables.command_line.open_the_multi_os_terminal %}
2. リポジトリのミラーしたベアクローンを作成します。
  ```shell
  $ git clone --mirror https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>repository-to-mirror</em>.git
  ```
3. プッシュの場所をミラーに設定します。
  ```shell
  $ cd <em>repository-to-mirror</em>
  $ git remote set-url --push origin https://{% data variables.command_line.codeblock %}/<em>exampleuser</em>/<em>mirrored</em>
  ```
ベアクローンと同様に、ミラーしたクローンにはすべてのリモートブランチとタグが含まれますが、フェッチするたびにすべてのローカルリファレンスが上書きされるため、常に元のリポジトリと同じになります。 プッシュする URL を設定することで、ミラーへのプッシュが簡素化されます。

4. ミラーを更新するには、更新をフェッチしてプッシュします。
  ```shell
  $ git fetch -p origin
  $ git push --mirror
  ```
{% ifversion fpt or ghec %}
## 参考リンク

* "[Pushing changes to GitHub](/desktop/contributing-and-collaborating-using-github-desktop/making-changes-in-a-branch/pushing-changes-to-github#pushing-changes-to-github)"
* "[About Git Large File Storage and GitHub Desktop](/desktop/getting-started-with-github-desktop/about-git-large-file-storage-and-github-desktop)"
* 「[GitHub Importer について](/github/importing-your-projects-to-github/importing-source-code-to-github/about-github-importer)」

{% endif %}
