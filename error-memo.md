# エラー＆躓き

## ``Service 'web' failed to build: The command '/bin/sh -c bundle install' returned a non-zero code: 20``
工程の``docker-compose.yml``まで作成し、``$ docker-compose run web rails new . --force --no-deps --database=postgresql``を実行した際のエラーに対処した際の備忘録です。

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
の前行にbundlerをインストールする命令を追加します。  
``RUN gem install bundler``

~~~dockerfile:変更後
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN gem install bundler
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

### $ docker-compose run web rails new . --force --no-deps --database=postgresql

実行後、下記のログが表示される。

~~~yml
Resolving dependencies...
Bundler could not find compatible versions for gem "sprockets":
  In snapshot (Gemfile.lock):
    sprockets (= 4.0.0)

  In Gemfile:
    sass-rails (~> 5.0) was resolved to 5.1.0, which depends on
      sprockets (>= 2.8, < 4.0)

    rails (~> 5.2.3) was resolved to 5.2.3, which depends on
      sprockets-rails (>= 2.0.0) was resolved to 3.2.1, which depends on
        sprockets (>= 3.0.0)

Running `bundle update` will rebuild your snapshot from scratch, using only
the gems in your Gemfile, which may resolve the conflict.
         run  bundle exec spring binstub --all
bundler: command not found: spring
Install missing gem executables with `bundle install`
~~~

下記を実行します。  
``$ docker-compose run web bundle update``  

(以下``bundle update``実行時のメッセージ、参考までにメモしておきます。)
~~~
Post-install message from chromedriver-helper:

  +--------------------------------------------------------------------+
  |                                                                    |
  |  NOTICE: chromedriver-helper is deprecated after 2019-03-31.       |
  |                                                                    |
  |  Please update to use the 'webdrivers' gem instead.                |
  |  See https://github.com/flavorjones/chromedriver-helper/issues/83  |
  |                                                                    |
  +--------------------------------------------------------------------+

Post-install message from sass:

Ruby Sass has reached end-of-life and should no longer be used.

* If you use Sass as a command-line tool, we recommend using Dart Sass, the new
  primary implementation: https://sass-lang.com/install

* If you use Sass as a plug-in for a Ruby web framework, we recommend using the
  sassc gem: https://github.com/sass/sassc-ruby#readme

* For more details, please refer to the Sass blog:
  https://sass-lang.com/blog/posts/7828841
~~~