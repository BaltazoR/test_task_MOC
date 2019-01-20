# /path/to/app/Dockerfile
FROM ruby:2.5-alpine

# Setting the time zone
RUN apk add --update tzdata && \
    cp /usr/share/zoneinfo/Europe/London /etc/localtime && \
    echo "Europe/London" > /etc/timezone

# Installing the runtime dependencies of the application in the container
RUN apk add --update --virtual runtime-deps postgresql-client nodejs libffi-dev readline sqlite

# We'll put everything in the temporary directory
WORKDIR /tmp
ADD Gemfile* ./

RUN apk add --virtual build-deps build-base libressl-dev postgresql-dev libc-dev linux-headers libxml2-dev libxslt-dev readline-dev && \
    bundle install --jobs=2 && \
    apk del build-deps

# Copy application code to container
ENV APP_HOME /app
COPY . $APP_HOME
WORKDIR $APP_HOME

# Setting environment variables for production
ENV RAILS_ENV=production \
    RACK_ENV=production

# Port forwarding 3000
EXPOSE 3000

# Running the default puma server
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
