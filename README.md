# aws-oidc-github-actions
GitHub ActionsからOIDCでAWSへ接続します

## workflowの内容について補足

下記のように「パーミッション」の設定をしている箇所がある。

```yaml
permissions:
  id-token: write
  contents: read
```

この`permissions`は、GitHub Actions内部から `GITHUB_TOKEN` を使用する際の権限を意味している。

### Assigning permissions to jobs
https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs

> You can use permissions to modify the default permissions granted to the GITHUB_TOKEN, adding or removing access as required, so that you only allow the minimum required access. 

> permission を使用して、GITHUB_TOKENに付与されているパーミッションを変更できます。必要に応じてアクセスを追加または削除して、必要最小限のアクセスのみを許可することができます。

`permissions` を一切指定しなければ、GitHub ActionsのJOB内部で全ての権限が有効になる。
しかし、何か指定した場合はその権限に絞って実行することが出来る。

### id-tokenの権限

```yaml
permissions:
  id-token: write
```

この権限を入れないと、`aws-actions/configure-aws-credentials@v1` でロールやregionの設定が失敗する。

JOBの中で環境変数への書き出しを許可するためには、このパーミッションを入れなければならない。

### contentsの権限

```yaml
permissions:
  contents: read
```

この権限を入れないと、リポジトリのクローン（リポジトリの読み込み）ができなくなる。


https://github.blog/changelog/2021-04-20-github-actions-control-permissions-for-github_token/

> Setting the default to contents:read is sufficient for any workflows that simply need to clone and build. 


> contents:readを設定すれば、git cloneとビルドが必要なワークフローには十分です。


### 参考
https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services
