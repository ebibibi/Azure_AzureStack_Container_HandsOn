# Azure / AzureStack / Container

ハンズオン

---

## アジェンダ

- Azure/AzureStack利用者としての体験
  - サブスクリプション準備
  - 仮想マシン展開 - ブラウザ
  - 仮想マシン展開 - PowerShell, CLI
  - テンプレート
  - コンテナ
  - PaaS
  - Serverless
- AzureStackオペレーターとしての体験
  - 検討中

---

## Azure/AzureStack利用者としての体験

まずはAzureおよびAzure Stackを利用者として体験いただきます。すべてセルフサービスで利用可能であり、世界最先端のサービス群を簡単に利用することができます。

---

## Azureサブスクリプション準備

- Azureでのサブスクリプションの作成
  - (これはデモを見ていただくのみとします。)
  - EA契約のAzure上のアカウントに紐付いているユーザーで操作します。

---

## Azure Stackサブスクリプション準備

- AzureStack管理ポータルでのサブスクリプションの作成

---

## 仮想マシン展開 - ブラウザ

- AzureへのLinux仮想マシンの展開
- AzureStackへのLinux仮想マシンの展開

---

## テンプレート

- 展開済みリソースのARMテンプレート確認
- (ARMテンプレート)Azure Quick Start Templateからの展開 - ブラウザ
- (ARMテンプレート)Azure Stack Quick Start Templateからの展開 - ブラウザ

---

## コンテナ

これから下記の操作を行います。

- 仮想マシン(Linux)をAzure CLIで展開
- コンテナでHello World
- コンテナでwhalesay
- イメージ作成
- Docker Hubにアップロード
- Wordpressの実行

---

## サブスクリプションをハンズオン用のものに設定

- ディレクトリを *windowsadmin* に変更
- Cloud Shell(Bash)を起動
- az account set --subscription *ハンズオンで使用するサブスクリプション名*
- az account show
  - 正しいサブスクリプションが設定されていることを確認

---

## コンテナ検証用VMをAzureに展開

- NAME=*自分の名前*
- az group create --name ${NAME}ContainerTest --location japaneast
- az vm create \
    --resource-group ${NAME}ContainerTest \
    --name ContainerTestVM \
    --image UbuntuLTS \
    --admin-username ${NAME} \
    --generate-ssh-keys
- (VMの作成を数分待ちます)
- 結果表示からpublicIPアドレスを確認します。

---

## 展開したVMにアクセス

- ssh ${NAME}@*確認したパブリックIPアドレス*
- "Are you sure you want to continue connection (yes/no)?"にyesで回答

---

## コンテナでHello World

- sudo apt install docker.io
- "Do you want to continue? [Y/n]" Y
- sudo docker container run hello-world
- 表示されたメッセージに注目

---

## コンテナイメージの確認

- sudo docker images

---

## whalesayイメージの取得～実行

- sudo docker pull docker/whalesay
- sudo docker images
- sudo docker run docker/whalesay cowsay Hello World!

---

## Dockerfile作成の準備(エディタ準備)

- sudo apt-get install emacs
- mkdir mybuild
- cd mybuild

---

## fortunes入りコンテナイメージ[mywhale]の定義

- emacs Dockerfile

```dockerfile
FROM docker/whalesay:latest
RUN apt-get -y update && apt-get install -y fortunes
CMD /usr/games/fortune -a | cowsay
```

- ファイル保存～終了(Ctrl-x,s -> Ctrl-x,c)

---

## fortunes入りコンテナイメージ[mywhale]の作成

- sudo docker build -t mywhale .
- sudo docker images

---

## fortunes入りコンテナイメージ[mywhale]の実行

- sudo docker run mywhale

---

## 作成したイメージのDockerHubへの公開

- sudo docker login
- sudo docker tag mywhale *自分のアカウント名*/mywhale
- sudo docker images
- sudo docker push *自分のアカウント名*/mywhale

---

## コンテナイメージの削除

- sudo docker rmi -f mywhale ebibibi/mywhale
- sudo docker images

---

## コンテナイメージのDockerHubからの取得と実行

- sudo docker pull ebibibi/mywhale
- sudo docker run ebibibi/mywhale

---

## コンテナ環境の削除

- sudo docker system prune
- "Are you sure you want to continue? [y/N]" Y

---

## docker-composeによる複数コンテナシステム

---

## docker-composeの準備

- sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- docker-compose version

---

## docker-compose.ymlの作成

- cd ~
- mkdir Wordpress
- cd Wordpress
- emacs docker-compose.yml

---

## docker-compose

```docker-compose
version: '3.1'

services:

  wordpress:
    image: wordpress
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_PASSWORD: example

  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
```

- ファイル保存～終了(Ctrl-x,s -> Ctrl-x,c)

---

## docker-composeによるイメージ取得

- sudo docker-compose pull
- sudo docker images

---

## docker-composeによるコンテナ実行(mysql)

- sudo docker-compose up -d mysql
- sudo docker-compose ps
- sudo docker-compose logs -f

---

## docker-composeによるコンテナ実行(wordpress全体)

- sudo docker-compose up -d
- sudo docker-compose logs -f wordpress

---

## NSG(Network Security Group)へのルール追加

- exit
- az network nsg rule create -g ${NAME}ContainerTest --nsg-name ContainerTestVMNSG  --name AllowHTTP--destination-address-prefix '*' --destination-port-ranges 80 --protocol TCP --priority 2000

---

## Wordpressの表示

- ブラウザで仮想マシンが保持するパブリックIPアドレスにアクセス
- (az network public-ip list | grep ipAddress)

---

## docker-composeによるコンテナ停止

- ssh ${NAME}@*確認したパブリックIPアドレス*
- cd Wordpress
- sudo docker-compose stop
- sudo docker-compose down
- sudo docker-compose ps

---

## 環境の削除

- sudo docker system prune
- "Are you sure you want to continue? [y/N]" Y
- exit

---

## リソースグループの削除

- az group delete --name ${NAME}ContainerTest -y

---

## PaaS

- AzureへのWebサイトの作成
- AzureStackへのへのWebサイトの作成
- SQL Databaseの作成
- Wordpressサイトの作成

---

## Serverless

- FunctionsでTeamsにメッセージを送信

---

## AzureStackオペレーターとしての体験

Azure StackにはAzureにはない特徴としてAzure Stackオペレーター用の管理ポータルがありオペレーション業務が行えます。これはAzure Stackがオンプレミスに存在し用途に合わせてカスタマイズできる基盤であるためです。
