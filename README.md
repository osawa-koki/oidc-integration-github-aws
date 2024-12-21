# oidc-integration-github-aws

👺👺👺 OIDCを利用してGitHubとAWSの連携して、GitHub ActionsでAWSのリソースを操作してみる！  

## デプロイ

```bash
source .env

aws cloudformation deploy \
  --template-file ./template.yml \
  --stack-name oidc-integration-github-aws \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameter-overrides GitHubUser=${GITHUB_USER}
```

各種パラメタは以下のコマンドで取得できます。  

```bash
source .env

aws cloudformation describe-stacks \
  --stack-name ${STACK_NAME} \
  --query 'Stacks[0].Outputs[*].{Key:OutputKey,Value:OutputValue}' \
  --output table
```

試しに、GitHub Actionsを実行してみましょう。  
以下のシークレットをGitHubのリポジトリに設定してください。  

| シークレット名 | 値 |
| --- | --- |
| AWS_ROLE_ARN | `GitHubOIDCRoleArn`の値 |

## おおさわメモ

GUIでこれらのリソースを作成する場合のメモです！  

### IDプロバイダーの作成

「IAM」ページの「ID プロバイダ」メニューから、「プロバイダを追加」をクリックします。  

![IDプロバイダー一覧](./images/id-providers-list-before-create.png)  

「プロバイダのタイプ」で「OpenID Connect」を選択します。  
その他の項目は以下の通りです。  

| 項目 | 値 |
| --- | --- |
| プロバイダのURL | `https://token.actions.githubusercontent.com` |
| 対象者 | `sts.amazonaws.com` |

![IDプロバイダーの作成](./images/id-provider-setting.png)  

その他の項目はデフォルトのままでOKです。  
そのまま作成します。  

一覧画面に戻ると、作成したIDプロバイダーが表示されていることが確認できます。  

![IDプロバイダー一覧](./images/id-providers-list-after-create.png)  

