This repository contains all the code needed to generate the website at https://www.mhdadk.com/. The style of this website is inspired by https://gregorygundersen.com/.

To build this website using Jekyll, run the following commands:
```sh
git clone git@github.com:mhdadk/mhdadk.github.io.git
cd mhdadk.github.io
bundle install
bundle exec jekyll serve
```
To see this website in action, open the `Server address` shown in the output of the
`bundle exec jekyll serve` command, which is usually `http://127.0.0.1:4000`, in a web browser.

After building the website, two directories should appear in the root directory of the
repository: `_site` and `.jekyll-cache`.

The following is an explanation for the purpose of each directory/file in this repository:

* `.github/workflows/jekyll.yml`: Configuration file for the GitHub Action used to
automatically build the website on each push to the repository.
* `_data/navigation.yml`: Pages included in the navigation bar.
* `_drafts`: Draft posts that are not yet ready for publishing. See `_drafts/README.md` for details.
* `_includes/mathjax.html`: Javascript to be included to enable math rendering via MathJax.
* `_includes/navigation.html`: Create the navigation bar dynamically using Liquid.
* `_layouts/default.html`: Default Jekyll [layout](https://jekyllrb.com/docs/step-by-step/04-layouts/) used on all pages.
* `_layouts/post.html`: Jekyll layout for posts.
* `_posts`: All blog posts. Each blog post file must have the format `YYYY-MM-DD-<title>.md` where `<title>` contains only dashes and alphanumerical characters.
* `assets/css`: All css files used for styling.
* `_site`: The actual site files that are displayed in the web browser.
* `.jekyll-cache`: Cache for faster Jekyll builds.
* `Gemfile`: Gemfile that contains all gems that should be installed via `bundle install`.
* `_config.yml`: Jekyll [configuration file](https://jekyllrb.com/docs/configuration/).
* `blog.html`: Blog posts page.
* `favicon.ico`: Empty file used to avoid the `ERROR '/favicon.ico' not found.` error when running `bundle exec jekyll serve`.
* `index.html`: Home page.
* `robots.txt`: Configure web scraping.

> [!IMPORTANT]  
> If you edit any of the .css files under `assets/css`, be sure to minify them. That is,
> if you edit file `X.css`, then minify it to generate `X.min.css`, since `X.min.css` is
> the one that is used for the website, not `X.css`.