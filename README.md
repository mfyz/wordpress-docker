# WordPress Docker Compose

## Configuration

Edit the `.env` file to change the default IP address, MySQL root password and WordPress database name.

## Installation

Open a terminal and `cd` to the folder in which `docker-compose.yml` is saved and run:

```
docker-compose up -d
```

This creates two new folders next to your `docker-compose.yml` file.

* `wp-data` – used to store and restore database dumps
* `wp-app` – the location of your WordPress application

## Usage

### Starting containers

You can start the containers with the `up` command in daemon mode (by adding `-d` as an argument) or by using the `start` command:

- Start/Stop
  - `docker-compose start -d`
  - `docker-compose stop`
- To Remove containers
  - `docker-compose down` (use `-v` if you need to remove the database volume)
- Export database dump
  - `sh ./export.sh`

### Developing a Theme

Configure the volume to load the theme in the container in the `docker-compose.yml`:

```
volumes:
  - ./theme-name/:/var/www/html/wp-content/themes/theme-name
```

### Developing a Plugin

Configure the volume to load the plugin in the container in the `docker-compose.yml`:

```
volumes:
  - ./plugin-name/:/var/www/html/wp-content/plugins/plugin-name
```

### WP CLI

The docker compose configuration also provides a service for using the [WordPress CLI](https://developer.wordpress.org/cli/commands/).

Sample command to install WordPress:

```
docker-compose run --rm wpcli core install --url=http://localhost:6001 --title=test --admin_user=admin --admin_password=admin --admin_email=test@example.com --skip-email
```

For an easier usage you may consider adding an alias for the CLI:

```
alias wp="docker-compose run --rm wpcli"
```

#### Other useful wpcli commands

- Users
  - `wp user list`
  - `wp user create bob bob@example.com --role=author`
  - `wp user update 123 --display_name=Mary --user_pass=marypass`
  - `wp user delete 123 --reassign=567`
- Themes
  - `wp theme list`
  - `wp theme activate twentysixteen`
  - `wp theme install twentysixteen --activate`
- Plugins
  - `wp plugin list`
  - `wp plugin activate hello`
  - `wp plugin install bbpress --activate`
- Options/Meta
  - `wp option get siteurl`
  - `wp option add my_option foobar`
  - `wp option update my_option '{"foo": "bar"}' --format=json`
  - `wp option delete my_option`
- Posts/Pages
  - `wp post create --post_type=post --post_title='A sample post'`
  - `wp post update 123 --post_status=draft`
  - `wp post delete 123`
- Comments
  - `wp comment create --comment_post_ID=15 --comment_content="hello blog" --comment_author="wp-cli"`
  - `wp comment delete $(wp comment list --status=spam --format=ids)`
- DB
  - `echo "select now()" | wp db query`
  - `wp db export > export.sql`
  - `wp db import < export.sql`

