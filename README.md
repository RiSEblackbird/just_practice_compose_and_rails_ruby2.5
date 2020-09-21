# _Compose_and_rails_ruby2.5

(This repository is my own self-study document
)

## リポジトリの目的

- ``Ruby``及び``Rails``の開発環境を``Docker Compose``で構築する基礎学習
- 学習記録/自分用資料まとめ

## 主な参照先

1. [クィックスタート: Compose と Rails | Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/rails/)
2. [Language Guide: Ruby - CircleCI](https://circleci.com/docs/2.0/language-ruby/)
3. [Configuring Deploys - CircleCI](https://circleci.com/docs/2.0/deployment-integrations/)
    * 日本語(※翻訳中)：[デプロイの構成 - CircleCI](https://circleci.com/docs/ja/2.0/deployment-integrations/)

## 工程(仮)
参照先のドキュメントの工程を基準にしつつ、手元環境に合わせて内容を必要最小限に調整しながら進める。
### 基準：[クィックスタート: Compose と Rails | Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/rails/)
* 作成：``.gitignore`` ([gitignore/Rails.gitignore](https://github.com/github/gitignore/blob/master/Rails.gitignore))
* 作成：``Dockerfile``
  * (bundlerのエラーへの対処するためにBundler 2系をインストール命令を追記する
``RUN gem install bundler``)
* 作成：``Gemfile``
* 作成：``Gemfile.lock``
* 作成：``entrypoint.sh``
* 作成：``docker-compose.yml``
* ``$ docker-compose run web rails new . --force --no-deps --database=postgresql``
* ``$ docker-compose run web bundle update``
* ``$ docker-compose build``
* ``config/database.yml``の記述を[クイックスタートのDB設定の記述](https://matsuand.github.io/docs.docker.jp.onthefly/compose/rails/#connect-the-database)に変更
* ``$ docker-compose up``
  * 起動中、別窓にて下記を実行。
  * ``$ docker-compose run web rake db:create``
  * ``http://localhost:3000``へアクセスしてRailsのデフォルトページの表示を確認。次へ。

### 基準:[Language Guide: Ruby - CircleCI](https://circleci.com/docs/2.0/language-ruby/)

## 確認用コマンド

### $ docker images

* すべてのトップレベルのイメージ、そのリポジトリとタグ、サイズを表示する。([docker images | Docker Documentation](https://docs.docker.com/engine/reference/commandline/images/))
    ~~~
    $ docker images
    REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
    compose_and_rails_ruby25_web   latest              086d86357626        2 minutes ago       1.07GB
    <none>                         <none>              90b935c35ff4        11 minutes ago      1.07GB
    <none>                         <none>              576f332d622f        9 hours ago         1.02GB
    postgres                       latest              4b52913b0a3a        12 days ago         313MB
    ruby                           2.5                 5ce41def636c        12 days ago         843MB
    ~~~

### $ docker ps

* コンテナを表示する。([ps — Docker-docs-ja 17.06 ドキュメント](https://docs.docker.jp/engine/reference/commandline/ps.html))
    ~~~
    $ docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    162d01c90e94        postgres            "docker-entrypoint.s…"   14 hours ago        Up 14 hours         5432/tcp            compose_and_rails_ruby25_db_1
    ~~~

    ~~~
    $ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
    4bf8aa2a7608        576f332d622f        "entrypoint.sh bundl…"   59 minutes ago      Exited (127) 59 minutes ago                          compose_and_rails_ruby25_web_run_5c6fc7c701b0
    8f50b029e4b1        576f332d622f        "entrypoint.sh bundl…"   About an hour ago   Exited (0) About an hour ago                         compose_and_rails_ruby25_web_run_4da1acbd1498
    0250b0f59fd1        576f332d622f        "entrypoint.sh bundl…"   About an hour ago   Exited (127) About an hour ago                       compose_and_rails_ruby25_web_run_d74efa3da769
    cf13c2e7b445        576f332d622f        "entrypoint.sh bundl…"   About an hour ago   Exited (0) About an hour ago                         compose_and_rails_ruby25_web_run_9d7258b9433b
    5b3a079ab7f2        576f332d622f        "entrypoint.sh rails…"   About an hour ago   Exited (0) About an hour ago                         compose_and_rails_ruby25_web_run_2701e900f28d
    2a9950555af6        576f332d622f        "entrypoint.sh rails…"   9 hours ago         Exited (0) 9 hours ago                               compose_and_rails_ruby25_web_run_63fbcb7dddbb
    06c35006e9c2        576f332d622f        "entrypoint.sh rails…"   9 hours ago         Exited (7) 9 hours ago                               compose_and_rails_ruby25_web_run_ebcb29dde571
    1f24e11fdf2a        576f332d622f        "entrypoint.sh rails…"   9 hours ago         Exited (0) 9 hours ago                               compose_and_rails_ruby25_web_run_466a8428eab3
    1a577509339b        9c0327c251e3        "/bin/sh -c 'bundle …"   12 hours ago        Exited (20) 12 hours ago                             pedantic_yonath
    9a8ed8d14f1e        9c0327c251e3        "/bin/sh -c 'bundle …"   13 hours ago        Exited (20) 13 hours ago                             confident_yonath
    694af0161ea7        9c0327c251e3        "/bin/sh -c 'bundle …"   14 hours ago        Exited (20) 14 hours ago                             determined_cori
    162d01c90e94        postgres            "docker-entrypoint.s…"   14 hours ago        Up 14 hours                      5432/tcp            compose_and_rails_ruby25_db_1
    ~~~
