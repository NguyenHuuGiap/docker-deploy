## Dockerfile

# Chọn image môi trường ruby
FROM ruby:2.5.1

# ENV
ENV NODE_VERSION 10

#Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - && \
  apt-get install -y nodejs && \
  rm -rf /var/lib/apt/lists/*

# Install packages
RUN apt-get update \
  && apt-get install -y \
  zlib1g-dev \
  build-essential \
  libssl-dev \
  libreadline-dev \
  libyaml-dev \
  libxml2-dev \
  libxslt1-dev \
  libcurl4-openssl-dev \
  libffi-dev \
  libpq-dev \
  git-core \
  vim \
  imagemagick

RUN apt-get update -qq && \
  apt-get install -y build-essential \
  xvfb \
  redis-tools && \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  lsof

#Clear lib
RUN apt-get clean autoclean \
  && apt-get autoremove -y \
  && rm -rf \
    /var/lib/apt \
    /var/lib/dpkg \
    /var/lib/cache \
    /var/lib/log

#Create workspace
ENV APP_ROOT /app/base
RUN mkdir -p $APP_ROOT
WORKDIR $APP_ROOT

ENV RAILS_ENV='production'
ENV RAKE_ENV='production' 

#Clone gemfile
COPY Gemfile /app/base
COPY Gemfile.lock /app/base
RUN gem install bundler -v 1.17.3 && bundler -v && bundle install --jobs 20 --retry 5 --without development test 

# COPY config/puma.rb /app/base/config/puma.rb

COPY . .

# RUN bundle exec rake assets:precompile
EXPOSE 3000
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
