# Docker drupal base

Template to install via docker a full configured drupal installation for developer's use.

**Table of contents:**

- [Requirements](#requirements)
- [Installation](#installation)
- [General information](#general_information)
  - [Admin user](#admin_user)
- [Functionality](#functionality)
  - [Faceter Functionality](#faceter_functionality)
- [Add new Functionality](#add_new_functionality)

## Requirements

This depends on the following software:

* [PHP 7.2 or 7.3](http://php.net/)
* [Docker](https://www.docker.com/)

## Installation

To start, run:

```bash
docker-compose up
```

It's advised to not daemonize `docker-compose` so you can turn it off (`CTRL+C`) quickly when you're done working.
However, if you'd like to daemonize it, you have to add the flag `-d`:

```bash
docker-compose up -d
```

Then:

```bash
docker-compose exec web composer install
docker-compose exec web ./vendor/bin/run drupal:site-install
```

Using default configuration, the development site files should be available in the `web` directory and the development site
should be available at: [http://127.0.0.1:8080/web](http://127.0.0.1:8080/web).

## General information

### Admin user

After the installation the admin user and the password are `admin|admin`.

## Functionality

By default all functionalities will be inactive in the `runner.yml.dist` file. The developer will have to uncomment all the lines belonging to a functionality.

### Faceter functionality

This functionality allows to work with the following modules:

* [Facets](https://www.drupal.org/project/facets)
* [Facets forms](https://www.drupal.org/project/facets_form)

Uncomment all the lines at `runner.yml.dist` file after line:
```
# Faceter functionality.
```

This will:

* Activate modules:

  * search_api
  * search_api_db
  * search_api_db_defaults
  * facets
  * facets_form
  * faceter_faceter

* Apply configuration at `faceter_faceter` module which will create:

  * Test content type
  * Create Faceter index for Test content type
  * Create view for that index and content type

* Generate via drush 50 nodes of Test Content type.
* Index via drush the content.

Once all that installation and configuration tasks are finished the developer will have a path at `/faceter` in which the view will show the indexed elements for Test content type. From here he can create as many facets as needed.

## Add new Functionality

First of all run this commands:

```
./vendor/bin/drush en features -y
./vendor/bin/drush en features_ui -y
```

Or activate modules via UI:

* Go to Extend
* Filter by `features`
* Select modules Features and Features UI
* Click on Install

Then create all the necessary stuff (content types, taxonomy, views, etc) and configure all the necessary to have a functionality ready to be worked with. All has to be prefixed by a certain especial word (check the `faceter` example).

Once all is done, go to features:

* Directly going to path `/admin/config/development/features`.
* Via menu going to `Configuration > Development > Features`.

Go to the `Bundle` tab, and in selector Bundle, select `-- New --`.

In this page:

* Add a *Bundle name* (preferably to use same especial word used to prefix everything)
* Add a *Distribution description*.
* Check the *Include install profile*.

Then click on Save settings to save the new bundle.

Go to the `Features` tab and follow the instructions:

* Click on *Create new Feature* button.
* Fill in *Name* field.
* Fill in *Description* field with a meaninful description of the functionality that is going to be packaged.
* Select *Bundle* created before for this package.
* Add a basic version, i.e. `8.x-1.0`.
* Leave path as it is.

Then comes the most delicate part, the developer must set `Search` field with the prefix word, then check that all elements appearing as selected are those necessary to the functionality and select them to be added to the package.

Once all is done, the archive can be downloaded by clicking on `Download Archive` button.

The developer has to add the module to the `/modules` folder, add the necessary elements at:

* `runner.yml.dist` file.
* `composer.json` file.

And test that the new feature is installed and properly configured. After testing, the new elements should be pushed to git.
