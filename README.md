# Hashcode

Hashcode is a site to track the real-time results of the `#code2013` hashtag on
Twitter.

## ENV variables

To run the Hashcode app, you'll need some environment variables set.

These are:

- `DATABASE_URL` - Postgres URL to your database (e.g. postgres://localhost/code2013)
- `HASHCODE_CONSUMER_KEY` - Twitter app consumer key
- `HASHCODE_CONSUMER_SECRET` - Twitter app consumer key
- `HASHCODE_ACCESS_TOKEN` - Twitter app OAuth token
- `HASHCODE_ACCESS_TOKEN_SECRET` - Twitter app OAuth token secret

We're providing a `.env.example` file that includes these environment variables, copy it and name it as `.env`.

## Running

1. Install dependencies with Bundler - `bundle install`
2. Run the server - `bundle exec rackup -p 3000`

## Scripts

Hashcode has two background scripts used to generate data, `import_tweets` and
`generate_stats`. These should be run at periodic intervals (5-10 minutes) to
pull in new tweets and generate the most recent stats.

The `import_tweets` script pulls in up to 100 of the most recent tweets with the
hashtag, and adds them to the database. It only pulls in tweets that have been
made since the most recent tweet in the database.

The `generate_stats` script takes all the tweets existing in the database, and
uses them to generate language stats, which are then persisted in the `Stats`
table. This way, rather than generating stats from the tweets for every request,
we simply fetch the most recent saved stats and display that to the end user.
