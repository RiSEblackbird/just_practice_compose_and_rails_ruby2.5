# エラー＆躓き

## ``Service 'web' failed to build: The command '/bin/sh -c bundle install' returned a non-zero code: 20``
工程の``docker-compose.yml``まで作成し、``$ docker-compose run web rails new . --force --no-deps --database=postgresql``を実行した際のエラー。

### Dockerfile

~~~dockerfile:変更前
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
~~~

~~~ログ＆エラーメッセージの一部
Step 7/13 : RUN bundle install
 ---> Running in 1a577509339b
You must use Bundler 2 or greater with this lockfile.
ERROR: Service 'web' failed to build: The command '/bin/sh -c bundle install' returned a non-zero code: 20
~~~

Bundler 2系を使用するようにとの指示。  
``RUN bundle install``  
の前行に当該Gemをインストールを実行する命令を追加する。  
``RUN gem install bundler -v 2.0.1``

~~~dockerfile:変更後
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN gem install bundler -v 2.1.4
RUN bundle install
COPY . /myapp

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
~~~