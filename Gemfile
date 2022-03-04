# frozen_string_literal: true

source 'https://rubygems.org'

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

# Synchronize with https://pages.github.com/versions
ruby '>=2.7.1'

gem 'github-pages', group: :jekyll_plugins

gem "webrick", "~> 1.7"

# see Github issue #11

group :jekyll_plugins do
    gem 'jekyll-commonmark-ghpages'
  end
