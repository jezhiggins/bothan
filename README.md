[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](http://theodi.mit-license.org)

# Bothan

A simple platform for publishing metrics, both as JSON, and as embeddable visualisations and dashboards.

Bothan was an experiment by [the ODI](https://theodi.org/). *As of 2019, Bothan is no longer maintained*, but this source code remains available, as well as tutorials on deploying your [personal instance of Bothan to Heroku](https://bothan.io/get-started.html) and intergrating with [Zapier](https://bothan.io/tutorials.html)

There are also two software libraries that make interfacing with Bothan super easy:

* [Ruby](https://github.com/theodi/bothan.rb)
* [Node.js](https://github.com/theodi/bothan.js)

## Summary of features

Bothan is a Sinatra web app that provides a simple wrapper around MongoDB to allow storage of time series metrics.
Bothan provides a REST API for storing and retrieving time-series data, as well as human-readable views which can be customised and embedded in other sites, allowing it to be used for building dashboards. It is designed for open publication of metrics data and includes licensing metadata for simple generation of an [Open Data Certificate](https://trello.com/c/ELxxqSeT/24-open-data-certificate)
Read the Documentation for the [API here](https://bothan.io/api.html) 

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

Follow the [public feature roadmap for Bothan](https://trello.com/b/2xc7Q0kd/labs-public-toolbox-roadmap?menu=filter&filter=label:Bothan)

## Upgrading

Most updates should not require intervention, but manual steps are occasionally required. If you are experiencing deployment problems, see [the upgrade guide](UPGRADING.md).

## Development

### Requirements
ruby version 2.3.0p0

The application uses mongodb for data persistence

The application requires capybara-webkit for testing (for install instuctions see below)

### Environment variables

```
METRICS_API_USERNAME=foo
METRICS_API_PASSWORD=bar
METRICS_API_TITLE='ODI Metrics'
METRICS_API_DESCRIPTION='This API contains a list of all metrics collected by the Open Data Institute since 2013'
METRICS_API_LICENSE_NAME='Creative Commons Attribution-ShareAlike'
METRICS_API_LICENSE_URL='https://creativecommons.org/licenses/by-sa/4.0/'
METRICS_API_PUBLISHER_NAME='Open Data Institute'
METRICS_API_PUBLISHER_URL='http://theodi.org'
METRICS_API_CERTIFICATE_URL='https://certificates.theodi.org/en/datasets/213482/certificate'
PUSHER_URL=
```
See below for Pusher configuration instructions

### Specific Development Instructions

##### Pusher setup

1. Log in to https://pusher.com
2. Create a new application and call it something sensible
3. Select the ```App Keys``` tab and note the following values

```
PUSHER_APP_ID=
PUSHER_KEY=
PUSHER_SECRET=
```

You can create the `PUSHER_URL` variable required for your `.env` file by concatenating the above variables as follows:
`PUSHER_KEY`:`PUSHER_SECRET`@api-eu.pusher.com/apps/`PUSHER_APP_ID`

#### Capybara

To run bothan locally requires capybara-webkit. Specific instructions to get this running is as follows

https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit#homebrew

At present Capybara depends on Qt 5. This requires the full `Xcode`, rather than `xcode developer tools`, to be installed

### Database Configuration

Install mongo:  
    `brew install mongo redis` (if using brew)

make a data directory for mongo databases  
    `sudo mkdir -p /data/db`

change directory ownership so that mongodb can operate    
    `sudo chown -R $USERNAME /data/`

### Development: Running the full application locally

Checkout the repository 

run `mongod` to establish a database for persistence

run `brew install cmake` or `sudo apt-get install cmake`

run ```bundle``` in the checked out directory.

optional: run `rake demo:setup` to establish some demo metrics for use locally

The app is loaded via Rack Middleware.  
execute `bundle exec rackup config.ru` to start the application

### Tests

A MongoDB instance must be running prior to executing test suites (see steps above from Running the full application locally for installation)

Execute `mongod`

The entire suite of unit tests (`rspec`) and user features (`cucumber`) can be executed with the `rake` command

alternatively execute each suite separately with  

* for unit tests execute `bundle exec rspec`
* for Cucumber features execute `bundle exec cucumber`

### Rake Tasks

`rake demo:setup` will establish one metric of [each type](https://bothan.io/visualisations.html) that Bothan supports

## Deployment

### Deployment On Heroku

Bothan can deploy a personal instance to Heroku.

You can employ this as an alternative to running a full local dev instance if you couple your heroku instance with the heroku toolbelt.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)
