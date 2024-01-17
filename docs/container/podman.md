---
tags:
  - container
  - podman
---

# Podman を使ってみる

## インストール

### 本体のインストール

Ubuntu 22.04 の Apt リポジトリに置いてあるバージョンが結構古い（3.4.4）ため、Kubic project からインストールする

リポジトリ登録（xUbuntu_22.04 の更新が止まっているっぽいので、Debian_Testingを使う）

```bash
$ curl -fsSL https://download.opensuse.org/repositories/devel:kubic:libcontainers:unstable/Debian_Testing/Release.key \
  | gpg --dearmor \
  | sudo tee /etc/apt/keyrings/devel_kubic_libcontainers_unstable.gpg > /dev/null
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/devel_kubic_libcontainers_unstable.gpg]\
    https://download.opensuse.org/repositories/devel:kubic:libcontainers:unstable/Debian_Testing/ /" \
  | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:unstable.list > /dev/null
```

インストール

```bash
$ sudo apt update
$ sudo apt install -y podman
```

足りないライブラリを個別にインストールする

```bash
$ sudo apt install libgpgme11 libyajl2
```

### レジストリ名を省略した場合の取得先を変更

Docker のように、 Docker Hub (docker.io) のイメージをレジストリ名を省略して取得可能にする

/etc/containers/registries.conf

```text
unqualified-search-registries = ["docker.io"]
```

※ 複数のレジストリを指定すると、どのレジストリを検索するか聞かれて面倒なので、一旦 `docker.io` のみにする

### podman-compose をインストール

pip からインストールする

```bash
$ pip install podman-compose
```

## Pod の作成

1. `web-pod-publish` という名前の Pod を作成します
    ```bash
    $ podman pod create --name=web-pod-publish -p 8080:80
    ```
1. 作成した Pod がリストに表示されることを確認