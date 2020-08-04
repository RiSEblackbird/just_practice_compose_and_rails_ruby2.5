[クィックスタート: Compose と Rails | Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/rails/)

Railsのgitignore
　[gitignore/Rails.gitignore](https://github.com/github/gitignore/blob/master/Rails.gitignore)
　[gitignore/Rails.gitignore](https://github.com/github/gitignore/blob/master/Rails.gitignore)

## 工程(仮)
* 作成：``.gitignore``
* 作成：``Dockerfile``
  * (bundlerのエラーへの対処するためにBundler 2系をインストール命令を追記する
``RUN gem install bundler``)
* 作成：``Gemfile``
* 作成：``Gemfile.lock``
* 作成：``entrypoint.sh``
* 作成：``docker-compose.yml``
* ``$ docker-compose run web rails new . --force --no-deps --database=postgresql``
* ``$ docker-compose run web bundle update``