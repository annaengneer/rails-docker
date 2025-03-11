# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Run git clone
  
* RUN git checkout -b docker

* create  Dockerfile &  docker-compose.yml

* Edit the Dockerfile
  
 ```Docker:Dockerfile
FROM ruby:3.2.2
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /rails-docker
WORKDIR /rails-docker
ADD Gemfile /rails-docker/Gemfile
ADD Gemfile.lock /rails-docker/Gemfile.lock
RUN bundle install
ADD . /rails-docker
```

* Edit the docker-compose.yml
```Docker:docker-compose.yml
 services:
  db:
    image: postgres:12
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_PASSWORD=postgres
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/rails-docker
    environment:
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=postgres
    tty: true
    stdin_open: true 
    ports:
      - "3000:3000"
    depends_on:
      - db
```
  

*  Edit the database.yml
```Docker:database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: postgres
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
development:
  <<: *default
  database: postgres
test:
  <<: *default
  database: postgres
production:
  <<: *default
  database: postgres
  username: rails-docker
  password: <%= ENV["RAILSDOCKER_DATABASE_PASSWORD"] %>

```

* RUN docker-compose up

* In another terminal, run docker-compose run web rake db:migrate

* Started the app！

