Dandelion
=========

Incremental Git repository deployment for OS X and Linux.

Install
-------

Ensure that Ruby is installed, then run:

    $ gem install dandelion

Alternatively, you can build the gem yourself:

    $ git clone git://github.com/scttnlsn/dandelion.git
    $ cd dandelion
    $ rake install

Config
------

Configuration options are specified in a YAML file (by default, the root of your
Git repository is searched for a file named `dandelion.yml`).

Example:

```yaml
scheme: sftp
host: example.com
username: user
password: pass
path: path/to/deployment

exclude:
    - .gitignore
    - dandelion.yml
    - folder/

additional:
    - public/css/print.css
    - public/css/screen.css
    - public/js/main.js
```

Required:

 * `scheme` (the file transfer scheme, see below)

Optional:

* `path` (relative path from root of remote file tree, defaults to the root)
* `local_path` (relative path from root of local repository, defaults to repository root)
* `exclude` (list of files or directories to exclude from deployment, if `local_path` is set files are relative to that path)
* `additional` (additional list of files from your working directory that will be deployed)
* `revision_file` (remote file in which revision SHA is stored, defaults to .revision)

Each scheme also has additional required and optional configuration parameters (see below).

Schemes
-------

There is support for multiple backend file transfer schemes.  The configuration
must specify one of these schemes and the set of additional parameters required
by the given scheme.

**SFTP**: `scheme: sftp`

Required:

 * `host`
 * `username`
 * `password`

Optional:

 * `port` (defaults to 22)
 * `preserve_permissions` (defaults to true)

**FTP**: `scheme: ftp`

Required:

 * `host`
 * `username`
 * `password`

Optional:

 * `port` (defaults to 21)
 * `passive` (defaults to true)

**Amazon S3**: `scheme: s3`

Required:

 * `access_key_id`
 * `secret_access_key`
 * `bucket_name`

Usage
-----

From within your Git repository, run:

    $ dandelion deploy

This will deploy the local `HEAD` revision to the location specified in the config
file.  Dandelion keeps track of the currently deployed revision so that only files
which have been added/changed/deleted need to be transferred.

You can specify the revision you wish to deploy and Dandelion will determine which
files need to be transferred:

    $ dandelion deploy <revision>

For a more complete summary of usage options, run:

    $ dandelion -h
    Usage: dandelion [options] <command> [<args>]
        -v, --version                    Display the current version
        -h, --help                       Display this screen
            --repo=[REPO]                Use the given repository
            --config=[CONFIG]            Use the given configuration file

    Available commands:
        deploy
        status

Note that when specifying the repository or configuration file, the given paths
are relative to the current working directory (not the repository root).  To see
the options for a particular command, run:

    $ dandelion <command> -h
