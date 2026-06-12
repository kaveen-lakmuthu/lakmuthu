# Kaveen Lakmuthu - Technical Portfolio & Systems Log

This is the codebase and content repository for my personal technical portfolio and systems investigation blog, built with the [Hugo](https://gohugo.io) static site generator.

## Structure

* `content/` - Markdown files containing site content.
  * `pages/` - Flat pages (About, Services, Now, Contact).
  * `investigations/` - Root-cause technical investigations and debug files.
  * `projects/` - Summaries of personal projects and NDAs-abstracted professional work.
* `layouts/` - HTML templates defining the custom minimalist layouts.
* `static/` - Static files, including stylesheets and icons.

## Development

To run the site locally in development mode:

```bash
# Start the local Hugo server
./bin/hugo server
```

The site will be available at `http://localhost:1313/`.

## Deployment

Deployments are automated through GitHub Actions. Pushing to the `main` branch triggers the workflow defined in `.github/workflows/hugo.yml` which compiles the static site and deploys it to GitHub Pages.

## Licensing

This repository uses different licenses for the website code and its written content:

* **Theme and Layout Code**: All layouts, templates, configuration scripts, and CSS are licensed under the [MIT License](LICENSE).
* **Written Content**: All text, prose, structure, and code snippets embedded inside the articles (any files under the `content/` directory) are protected under the [Content License Agreement](CONTENT_LICENSE.md) (All Rights Reserved).
