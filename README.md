# Blog Escape
Copyright &copy; 2017 Bart Massey

Blog Escape is a database scraper for
[Drupal](http://drupal.org) sites. It digs the content out
of a site by scraping the database and deposits it in
well-formatted static files, ready for deployment elsewhere.
It was intended for my blog, but will try to rescue as much
of the Drupal site as possible.

Drupal breaks all the time. I used it as my blog site for
many years (Drupal 4 through Drupal 7), but eventually it
got to the point where it would neither run nor upgrade.
This software is my solution for getting my old blog content
back.

Blog Escape is written in Python 3 and currently targeted at
Drupal 7 Postgres content.

This is a work in progress. Its current functionality is
nonexistent, and its design is still in flux.

## How To Use

We will assume that the domain name of your sitename is
`drupal.example.org`.

1.  This software requires the Python 3 `MySQLDB`
    module. Install it from your distro or with `pip3`.
    It also requires the `mistune` Markdown parser.
    Same deal.

1.  You may want to create a fresh copy of the Drupal
    database, just to avoid accidents. This should not be
    necessary, but better safe than sorry. Use `mysql-dump`
    and then restore the database into its new home.

    **Danger:** your old Drupal database and the new one are
    probably both completely screwed up with respect to the
    MySQL `utf8`, `utf8mb4` and `latin1` encodings. Dig
    around in the excellent series of reference cards
    starting
    [here](https://makandracards.com/makandra/595-dumping-and-importing-from-to-mysql-in-an-utf-8-safe-way)
    to figure out how to get a copy of the database with
    Unicode preserved.

1.  Find the `files` directory under your site directory in
    your Drupal installation. Copy it over and `cd` into it.
    Use `mv -f images/* . ; rmdir images` to get all the old
    nested images out. Hope that there are no name
    collisions between images with different content. You
    can also remove the `files` → `.` symbolic link if you
    have one and would like.

1.  Create `drupal-example-org.my.cnf` with the username,
    database name and password of the Drupal database.
   
    ```
    [client]
    user="example_user"
    database="example_database"
    password="example_password"
    ```

1. Run `python3 blog-escape.py drupal.example.org`.

1. Move the resulting `nodes` and `files` directory to the
   root of your new archive website. Adjust your webserver
   configuration as needed.

## Useful Links

* [Drupal 7 DB doc](http://www.drupal.org/node/1785994#d7)
* [Drupal 7 DB content storage](http://drupal.stackexchange.com/a/6791)
* [MySQLdb doc](http://mysqlclient.readthedocs.io)
* [ATOM RFC](http://tools.ietf.org/html/rfc4287)

## Status

See the GitHub
[Development](https://github.com/BartMassey/blog-escape/projects/1)
Project Board for pending and completed development tasks.

## Limitations

* Right now, some of the details of content formats as
  encoded in the tool are pretty heuristic.  There
  should be a configurable mechanism for mapping content
  formats to extensions.

* Ideally, all of the Drupal extension filters should be
  replicated and applied in the specified order, but this
  would be a ton of work.

* Currently only works (read "has been tested") for Drupal
  sites that use a site-specific domain name for the root
  URL.

## License

This work is made available under the MIT license. Please
see the file `COPYING` in this distribution for license terms.
