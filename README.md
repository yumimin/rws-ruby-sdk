# RakutenWebService

[![Build Status](https://travis-ci.org/rakuten-ws/rws-ruby-sdk.png?branch=master)](https://travis-ci.org/rakuten-ws/rws-ruby-sdk)
[![Gem Version](https://badge.fury.io/rb/rakuten_web_service.png)](http://badge.fury.io/rb/rakuten_web_service)
[![Coverage Status](https://coveralls.io/repos/github/rakuten-ws/rws-ruby-sdk/badge.svg?branch=master)](https://coveralls.io/github/rakuten-ws/rws-ruby-sdk?branch=master)

This gem provides a client for easily accessing [Rakuten Web Service APIs](https://webservice.rakuten.co.jp/).

# Table of Contents

* [Prerequisite](#prerequisite)
* [Installation](#installation)
* [Usage](#usage)
  * [Prerequisite: Getting Application ID](#prerequisite-getting-application-id)
  * [Configuration](#configuration)
  * [Search Ichiba Items](#search-ichiba-items)
  * [Pagerizing](#pagerizing)
  * [Genre](#genre)
  * [Ichiba Item Ranking](#ichiba-item-ranking)
* [Supported APIS](#supported-apis)
  * [Rakuten Ichiba APIs](#rakuten-ichiba-apis)
  * [Rakuten Books APIs](#rakuten-books-apis)
  * [Rakuten Kobo APIs](#rakuten-kobo-apis)
  * [楽天レシピ系API](#%E6%A5%BD%E5%A4%A9%E3%83%AC%E3%82%B7%E3%83%94%E7%B3%BBapi)
  * [Rakuten GORA APIs](#rakuten-gora-apis)
* [Contributing](#contributing)


## Prerequisite

* Ruby 2.1.0 or later

## Installation

Add this line to your application's Gemfile:

```ruby
  gem 'rakuten_web_service'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rakuten_web_service


## Usage

### Prerequisite: Getting Application ID

You need to get Application ID for your application to access to Rakuten Web Service APIs. 
If you have not got it, register your application [here](https://webservice.rakuten.co.jp/app/create). 

### Configuration

At first, you have to specify your application's key. And you can tell the client your afiiliate id with `RakutenWebService.configuration`.

```ruby
  RakutenWebService.configuration do |c|
    c.application_id = 'YOUR_APPLICATION_ID'
    c.affiliate_id = 'YOUR_AFFILIATE_ID'
  end
```

Please note that you need to replace `'YOUR_APPLICATION_ID'` and `'YOUR_AFFILIATE_ID'` with actual ones you have.

### Search Ichiba Items

```ruby
  items = RakutenWebService::Ichiba::Item.search(:keyword => 'Ruby') # This returns Enumerable object
  items.first(10).each do |item|
    puts "#{item['itemName']}, #{item.price} yen" # You can refer to values as well as Hash.
  end
```

### Pagerizing

Responses of resources' `search` such as `RakutenWebService::Ichiba::Item.search` have methods for paginating fetched resources.

```ruby
  items = RakutenWebService::Ichiba::Item.search(keyword: 'Ruby')
  items.count #=> 30. In default, the API returns up to 30 items matched with given keywords.

  last_items = items.page(3) # Skips first 2 pages.

  # Go to the last page
  while last_items.has_next_page?
    last_items = last_items.next_page
  end

  # Shows the title of the last 30 items
  last_items.each do |item|
    puts item.name
  end

  items.all do |item|
    puts item.name
  end
```

### Genre

Genre class provides an interface to traverse sub genres.

```ruby
  root = RakutenWebService::Ichiba::Genre.root # root genre
  # children returns sub genres
  root.children.each do |child|
    puts "[#{child.id}] #{child.name}"
  end

  # Use genre id to fetch genre object
  RakutenWebService::Ichiba::Genre[100316].name # => "水・ソフトドリンク"
```


### Ichiba Item Ranking

```ruby
  ranking_by_age = RakutenWebService::Ichiba::Item.ranking(:age => 30, :sex => 1) # returns the TOP 30 items for Male in 30s
  # For attributes other than 'itemName', see: http://webservice.rakuten.co.jp/api/ichibaitemsearch/#outputParameter
  ranking_by_age.each do |ranking|
    puts ranking['itemName']
  end

  ranking_by_genre = RakutenWebService::Ichiba::Genre[200162].ranking　# the TOP 30 items in "水・ソフトドリンク" genre
  ranking_by_genre.each do |ranking|
    puts ranking['itemName']
  end
```

## Supported APIS

Now rakuten\_web\_service is supporting the following APIs:

### Rakuten Ichiba APIs

* [Rakuten Ichiba Item Search API](http://webservice.rakuten.co.jp/api/ichibaitemsearch/)
* [Rakuten Ichiba Genre Search API](http://webservice.rakuten.co.jp/api/ichibagenresearch/)
* [Rakuten Ichiba Ranking API](http://webservice.rakuten.co.jp/api/ichibaitemranking/)
* [Rakuten Product API](http://webservice.rakuten.co.jp/api/productsearch/)

### Rakuten Books APIs

* [Rakuten Books Total Search API](http://webservice.rakuten.co.jp/api/bookstotalsearch/)
* [Rakuten Books Book Search API](http://webservice.rakuten.co.jp/api/booksbooksearch/)
* [Rakuten Books CD Search API](http://webservice.rakuten.co.jp/api/bookscdsearch/)
* [Rakuten Books DVD/Blu-ray Search API](http://webservice.rakuten.co.jp/api/booksdvdsearch/)
* [Rakuten Books ForeignBook Search API](http://webservice.rakuten.co.jp/api/booksforeignbooksearch/)
* [Rakuten Books Magazine Search API](http://webservice.rakuten.co.jp/api/booksmagazinesearch/)
* [Rakuten Books Game Search API](http://webservice.rakuten.co.jp/api/booksgamesearch/)
* [Rakuten Books Software Search API](http://webservice.rakuten.co.jp/api/bookssoftwaresearch/)
* [Rakuten Books Genre Search API](http://webservice.rakuten.co.jp/api/booksgenresearch/)

### Rakuten Kobo APIs

* [Rakuten Kobo Ebook Search API](http://webservice.rakuten.co.jp/api/koboebooksearch/)
* [Rakuten Kobo Genre Search API](http://webservice.rakuten.co.jp/api/kobogenresearch/)

### 楽天レシピ系API

* [Rakuten Recipe Category List API](https://webservice.rakuten.co.jp/api/recipecategorylist/)
* [Rakuten Recipe Category Ranking API](https://webservice.rakuten.co.jp/api/recipecategoryranking/)

### Rakuten GORA APIs

* [Rakuten GORA Golf Course Search API](https://webservice.rakuten.co.jp/api/goragolfcoursesearch/)
* [Rakuten GORA Golf Course Detail Search API](https://webservice.rakuten.co.jp/api/goragolfcoursedetail/)
* [Rakuten GORA Plan Search API](https://webservice.rakuten.co.jp/api/goraplansearch/)

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
