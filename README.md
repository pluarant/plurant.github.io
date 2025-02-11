# Mesibo Website

This is the source for [https://mesibo.com/](https://mesibo.com/){: target="_blank"}.

Feel free to send us pull requests and file issues. Our docs are completely
open source and we deeply appreciate contributions from our community!

## Providing feedback

We really want your feedback, and we've made it easy.  You can edit a page or
request changes in the right column of every page on
[mesibo.com](https://mesibo.com/){: target="_blank"}.  You can also rate each page by
clicking a link at the footer.

**Only file issues about the documentation in this repository.** One way
to think about this is that you should file a bug here if your issue is that you
don't see something that should be in the docs, or you see something incorrect
or confusing in the docs.

- If your problem is a general question about how to use Mesibo,
  visit [https://mesibo.com/help/](https://mesibo.com/help/){: target="_blank"} instead.

- If you have an idea for a new feature or behavior change in a specific aspect
  of Mesibo, or have found a bug in part of Mesibo, file that issue in
  the project's code repository.

## Contributing

We value your documentation contributions, and we want to make it as easy
as possible to work in this repository. One of the first things to decide is
which branch to base your work on. If you get confused, just ask and we will
help. If a reviewer realizes you have based your work on the wrong branch, we'll
let you know so that you can rebase it.

>**Note**: To contribute code to Mesibo projects, see the
[Contribution guidelines](CONTRIBUTING.md).

## Staging the docs

You have two options:

1.  On your local machine, clone this repo and run our staging container:

    ```bash
    git clone --recursive https://github.com/mesibo/website.git
    cd website
    jekyll start
    ```

2.  Install Jekyll and GitHub Pages on your local machine.

    a. Clone this repo by running:

       ```bash
       git clone --recursive https://github.com/mesibo/website.git
       ```

    b. Install Ruby 2.3 or later as described in [Installing Ruby](https://www.ruby-lang.org/en/documentation/installation/){: target="_blank"}.

    c. Install Bundler:

       ```bash
       gem install bundler
       ```

    d. If you use Ubuntu, install packages required for the Nokogiri HTML
       parser:

       ```bash
       sudo apt-get install ruby-dev zlib1g-dev liblzma-dev
       ```

    e. Install Jekyll and other required dependencies:

       ```bash
       bundle install
       ```

       >**Note**: You may need to install some packages manually.   

    f. Change the directory to `docker.github.io`.

    g. Use the `jekyll serve` command to continuously build the HTML output.

    The `jekyll serve` process runs in the foreground, and starts a web server
    running on http://localhost:4000/ by default. To stop it, use `CTRL+C`.
    You can continue working in a second terminal and Jekyll will rebuild the
    website incrementally. Refresh the browser to preview your changes.

## Read these docs offline

To read the docs offline, you can use either a standalone container or a swarm service.
To see all available tags, go to
[Docker Hub](https://hub.docker.com/r/docs/docker.github.io/tags/){: target="_blank"}.
The following examples use the `latest` tag:

- Run a single container:

  ```bash
  docker run  -it -p 4000:4000 docs/docker.github.io:latest
  ```

- Run a swarm service:

  ```bash
  docker service create -p 4000:4000 --name localdocs --replicas 1 docs/docker.github.io:latest
  ```

  This example uses only a single replica, but you could run as many replicas as you'd like.

Either way, you can now access the docs at port 4000 on your Docker host.

## Important files

- `/_data/toc.yaml` defines the left-hand navigation for the docs
- `/js/menu.js` defines most of the docs-specific JS such as TOC generation and menu syncing
- `/css/documentation.css` defines the docs-specific style rules
- `/_layouts/docs.html` is the HTML template file, which defines the header and footer, and includes all the JS/CSS that serves the docs content

## Relative linking for GitHub viewing

Feel free to link to `../foo.md` so that the docs are readable in GitHub, but keep in mind that Jekyll templating notation
`{% such as this %}` will render in raw text and not be processed. In general it's best to assume the docs are being read
directly on [https://docs.docker.com/](https://docs.docker.com/){: target="_blank"}.

### Testing changes and practical guidance

If you want to test a style change, or if you want to see how to achieve a
particular outcome with Markdown, Bootstrap, JQuery, or something else, have
a look at `test.md` (which renders in the site at `/test/`).

### Per-page front-matter

The front-matter of a given page is in a section at the top of the Markdown
file that starts and ends with three hyphens. It includes YAML content. The
following keys are supported. The title, description, and keywords are required.

| Key                    | Required  | Description                             |
|------------------------|-----------|-----------------------------------------|
| title                  | yes       | The page title. This is added to the HTML output as a `<h1>` level header. |
| description            | yes       | A sentence that describes the page contents. This is added to the HTML metadata. |
| keywords               | yes       | A comma-separated list of keywords. These are added to the HTML metadata. |
| redirect_from          | no        | A YAML list of pages which should redirect to THIS page. At build time, each page listed here is created as a HTML stub containing a 302 redirect to this page. |
| notoc                  | no        | Either `true` or `false`. If `true`, no in-page TOC is generated for the HTML output of this page. Defaults to `false`. Appropriate for some landing pages that have no in-page headings.|
| toc_min                | no        | Ignored if `notoc` is set to `true`. The minimum heading level included in the in-page TOC. Defaults to `2`, to show `<h2>` headings as the minimum. |
| toc_max                | no        | Ignored if `notoc` is set to `false`. The maximum heading level included in the in-page TOC. Defaults to `3`, to show `<h3>` headings. Set to the same as `toc_min` to only show `toc_min` level of headings. |
| tree                   | no        | Either `true` or `false`. Set to `false` to disable the left-hand site-wide navigation for this page. Appropriate for some pages like the search page or the 404 page. |
| no_ratings             | no        | Either `true` or `false`. Set to `true` to disable the page-ratings applet for this page. Defaults to `false`. |

The following is an example of valid (but contrived) page metadata. The order of
the metadata elements in the front-matter is not important.

```liquid
---
description: Instructions for installing Docker on Ubuntu
keywords: requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/ubuntulinux/
- /installation/ubuntulinux/
- /engine/installation/linux/ubuntulinux/
title: Get Docker for Ubuntu
toc_min: 1
toc_max: 6
tree: false
no_ratings: true
---
```

### Creating tabs

The use of tabs, as on pages like [https://docs.docker.com/engine/api/](/engine/api/){: target="_blank"}, requires
the use of HTML. The tabs use Bootstrap CSS/JS, so refer to those docs for more
advanced usage. For a basic horizontal tab set, copy/paste starting from this
code and implement from there. Keep an eye on those `href="#id"` and `id="id"`
references as you rename, add, and remove tabs.

```
<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" data-target="#tab1">TAB 1 HEADER</a></li>
  <li><a data-toggle="tab" data-target="#tab2">TAB 2 HEADER</a></li>
</ul>
<div class="tab-content">
  <div id="tab1" class="tab-pane fade in active">TAB 1 CONTENT</div>
  <div id="tab2" class="tab-pane fade">TAB 2 CONTENT</div>
</div>
```

For more info and a few more permutations, see `test.md`.

### Running in-page Javascript

If you need to run custom Javascript within a page, and it depends upon JQuery
or Bootstrap, make sure the `<script>` tags are at the very end of the page,
after all the content. Otherwise the script may try to run before JQuery and
Bootstrap JS are loaded.

> **Note**: In general, this is a bad idea.

### Images

Don't forget to remove images that are no longer used.  Keep the images sorted
in the local `images/` directory, with names that naturally group related images
together in alphabetical order.  For instance prefer `settings-file-share.png`
and `settings-proxies.png` to `file-share-settings.png` and
`proxies-settings.png`.  You may also use numbers, especially in the case of a
sequence, e.g., `run-only-the-images-you-trust-1.svg`
`run-only-the-images-you-trust-2.png` `run-only-the-images-you-trust-3.png`.

When applicable, capture windows rather than rectangular regions.  This
eliminates unpleasant background and saves the editors the need to crop.

On Mac, capture windows without shadows.  To this end, once you pressed
`Command-Shift-4`, press Option while clicking on the window.  To disable
shadows once for all, run:

```bash
$ defaults write com.apple.screencapture disable-shadow -bool TRUE
$ killall SystemUIServer  # restart it.
```

You can restore shadows later with `-bool FALSE`.

In order to keep the Git repository light, _please_ compress the images
(losslessly).  On Mac you may use (ImageOptim)[https://imageoptim.com] for
instance.  Be sure to compress the images *before* adding them to the
repository, doing it afterwards actually worsens the impact on the Git repo (but
still optimizes the bandwidth during browsing).

## Beta content disclaimer
```bash
> BETA DISCLAIMER
>
> This is beta content. It is not yet complete and should be considered a work in progress. This content is subject to change without notice.
```

## Building archives and the live published docs

All the images described below are automatically built using Docker Hub. To
build the site manually, from scratch, including all utility and archive images,
see the [README in the publish-tools
branch](https://github.com/docker/docker.github.io/blob/publish-tools/README.md){: target="_blank"}.

- Some utility images are built from Dockerfiles in the `publish-tools` branch.
  See its [README](https://github.com/docker/docker.github.io/blob/publish-tools/README.md){: target="_blank"}
  for details.
- Each archive branch automatically builds an image tagged
  `docs/docker.github.io:v<VERSION>` when a change is merged into that branch.
- The `master` branch has a Dockerfile which uses the static HTML from each
  archive image, in combination with the Markdown
  files in `master` and some upstream resources which are fetched at build-time,
  to create the full site at [https://docs.docker.com/](/){: target="_blank"}. All
  of the long-running branches, such as `vnext-engine`, `vnext-compose`, etc,
  use the same logic.

## Creating a new archive

When a new Docker CE Stable version is released, the previous state of `master`
is archived into a version-specific branch like `v17.09`, by doing the following:

1.  Create branch based off the commit hash before the new version was released.

    ```bash
    $ git checkout <HASH>
    $ git checkout -b v17.09
    ```

2.  Run the `_scripts/fetch-upstream-resources.sh` script. This puts static
    copies of the files in place that the `master`  build typically fetches
    each build.

    ```bash
    $ _scripts/fetch-upstream/resources.sh
    ```

3.  Overwrite the `Dockerfile` with the `Dockerfile.archive` (use `cp` rather
    than `mv` so you don't inadvertently remove either file). Edit the resulting
    `Dockerfile` and set the `VER` build argument to the appropriate value, like
    `v17.09`.

    ```bash
    $ mv Dockerfile.archive Dockerfile
    $ vi Dockerfile

      < edit the variable and save >
    ```

4.  Do `git status` and add all changes, being careful not to add anything extra
    by accident. Commit your work.

    ```bash
    $ git status
    $ git add <filename>
    $ git add <filename> (etc etc etc)
    $ git commit -m "Creating archive for 17.09 docs"
    ```

5.  Make sure the archive builds.

    ```bash
    $ docker build -t docker build -t docs/docker.github.io:v17.09 .
    $ docker run --rm -it -p 4000:4000 docs/docker.github.io:v17.09
    ```

    After the `docker run` command, browse to `http://localhost:4000/` and
    verify that the archive is self-browseable.

6.  Push the branch to the upstream repository. Do not create a pull request
    as there is no reference branch to compare against.

    ```bash
    $ git push upstream v17.09
    ```

## Copyright and license

Code and documentation copyright 2017 Docker, inc, released under the Apache 2.0 license.
