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
