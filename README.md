# AWS Secrets Manager メモ

Twitter APIのキー4つをlambdaに埋め込まない、
という件で調査したときのサンプルコード。

# リンク

- [AWS Secrets Manager とは ? - AWS Secrets Manager](https://docs.aws.amazon.com/ja_jp/secretsmanager/latest/userguide/intro.html)
- [チュートリアル : シークレットの保存と取得 - AWS Secrets Manager](https://docs.aws.amazon.com/ja_jp/secretsmanager/latest/userguide/tutorials_basic.html)
- [AWS Secrets Managerを使ってLambdaのシークレットを管理する ｜ Developers.IO](https://dev.classmethod.jp/cloud/aws/lambda-blueprint-slack-cloudwatch-with-secrets-manager/)
- [AWS CloudFormation のシークレット作成を自動化する - AWS Secrets Manager](https://docs.aws.amazon.com/ja_jp/secretsmanager/latest/userguide/integrating_cloudformation.html)


# 作業

まずポータルの[Secrets Manager](https://console.aws.amazon.com/secretsmanager/)を使って作ってみる。

手順は [チュートリアル : シークレットの保存と取得 - AWS Secrets Manager](https://docs.aws.amazon.com/ja_jp/secretsmanager/latest/userguide/tutorials_basic.html) に従う。

肝は
シークレットシークレット名
に
tutorials/MyFirstTutorialSecret
とつけるだけ(あとリージョン)。


「確認」のところで出るサンプルコード(自分の使いそうなやつだけ)はexampleフォルダにコピーした。
- Python3
- Node.js
- GoLang

あと[チュートリアル : シークレットの保存と取得 - AWS Secrets Manager](https://docs.aws.amazon.com/ja_jp/secretsmanager/latest/userguide/tutorials_basic.html)にあるシェル

```sh
aws secretsmanager describe-secret --secret-id tutorials/MyFirstTutorialSecret
aws secretsmanager get-secret-value --secret-id tutorials/MyFirstTutorialSecret --version-stage AWSCURRENT
```

これらのうちからPython3版をちょびっと改造したのが
ex1.py



えらいプロファイルだと、プログラムからでも、ポータル
[Secrets Manager](https://console.aws.amazon.com/secretsmanager/home#/listSecrets)
からでもシークレットが見える。

上のシェルの2つめのコードで、
限定された権限のプロファイルでシークレットが見えるか試す。

```
$ aws secretsmanager get-secret-value --profile yowai --secret-id tutorials/MyFirstTutorialSecret --version-stage AWSCURRENT

An error occurred (AccessDeniedException) when calling the GetSecretValue operation: User: arn:aws:iam::XXXXXXXXXXXXX:user/elbtest is not authorized to perform: secretsmanager:GetSecretValue on resource: arn:aws:secretsmanager:ap-northeast-1:XXXXXXXXXXXXX:secret:tutorials/MyFirstTutorialSecret-XXXXXX
```

読める権限はどこで設定するのか。
lambda編に続く。
