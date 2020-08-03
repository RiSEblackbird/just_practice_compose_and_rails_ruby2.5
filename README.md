#

## Docker, Compose
[クィックスタート: Compose と Rails | Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/compose/rails/)

Railsのgitignore
　[gitignore/Rails.gitignore](https://github.com/github/gitignore/blob/master/Rails.gitignore)

## 工程(仮)
1. 作成：``.gitignore``
2. 作成：``Dockerfile``
3. 作成：``Gemfile``
4. 作成：``Gemfile.lock``
5. 作成：``entrypoint.sh``
6. 作成：``docker-compose.yml``
7. ``$ docker-compose run web rails new . --force --no-deps --database=postgresql``
