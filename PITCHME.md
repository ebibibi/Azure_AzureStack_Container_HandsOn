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

### サブスクリプション準備

- Azureでのサブスクリプションの作成
- AzureStackでのサブスクリプションの作成

#### 仮想マシン展開 - ブラウザ

- AzureへのLinux仮想マシンの展開
- AzureStackへのLinux仮想マシンの展開

#### 仮想マシン展開 - PowerShell, CLI

- Azureへの仮想マシンの展開 - PowerShell
- AzureStackへの仮想マシンの展開 - PowerShell
- Azureへの仮想マシンの展開 - CLI
- AzureStackへの仮想マシンの展開 - CLI
- Azure CloudShell

#### テンプレート

- 展開済みリソースのARMテンプレート確認
- (ARMテンプレート)Azure Quick Start Templateからの展開 - ブラウザ
- (ARMテンプレート)Azure Stack Quick Start Templateからの展開 - ブラウザ
- (Terraform)Terraformでの展開

#### コンテナ

- コンテナでHello World
- コンテナでwhalesay
- イメージ作成
- Docker Hubにアップロード
- Wordpressの実行

#### PaaS

- AzureへのWebサイトの作成
- AzureStackへのへのWebサイトの作成
- SQL Databaseの作成
- Wordpressサイトの作成

#### Serverless

- FunctionsでTeamsにメッセージを送信

## AzureStackオペレーターとしての体験

## Container

- Ubuntu Server 18.04 LTSを展開
- sudo apt install docker.io
- sudo docker container run hello-world
- sudo docker images
- sudo docker pull docker/whalesay
- sudo docker images
- sudo docker run docker/whalesay cowsay Hello World!
- sudo apt install emacs
- mkdir mybuild
- cd mybuild
- emacs Dockerfile

```dockerfile
FROM docker/whalesay:latest
RUN apt-get -y update && apt-get install -y fortunes
CMD /usr/games/fortune -a | cowsay
```

- sudo docker build -t mywhale .
- sudo docker images
- sudo docker run mywhale
- sudo docker login
- sudo docker tag mywhale ebibibi/mywhale
- sudo docker images
- sudo docker push ebibibi/mywhale
- sudo docker rmi -f mywhale ebibibi/mywhale
- sudo docker run ebibibi/mywhale
- sudo docker pull ebibibi/mywhale
- sudo docker run ebibibi/mywhale
- sudo docker system prune

- sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- docker-compose version
- mkdir Wordpress
- cd Wordpress
- emacs docker-compose.yml

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

- sudo docker-compose pull
- sudo docker images
- sudo docker-compose up -d mysql
- sudo docker-compose ps
- sudo docker-compose logs -f
- sudo docker-compose up -d
- sudo docker-compose logs -f wordpress
- sudo docker-compose stop
- sudo docker-compose down
- sudo docker-compose ps

