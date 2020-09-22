FROM ruby:2.7.1

RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs npm

ENV CODE_ROOT /var/www/app/
ENV RAILS_ROOT /var/www/app/webapp/
RUN mkdir -p $CODE_ROOT
RUN mkdir -p $RAILS_ROOT

ENV RAILS_ENV='development'
ENV RACK_ENV='development'

WORKDIR $RAILS_ROOT

ENV BUNDLE_PATH /box

COPY Gemfile Gemfile.lock package.json yarn.lock $RAILS_ROOT
COPY Gemfile* $RAILS_ROOT
RUN bundle install

ADD . $RAILS_ROOT
RUN npm uninstall -g npm
RUN npm i -g npm@next
RUN npm install -g yarn
RUN yarn install --check-files
RUN bundle exec rake assets:precompile  --trace

EXPOSE 3000
CMD ["bundle", "exec", "rails", "webpacker:install"]
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]

