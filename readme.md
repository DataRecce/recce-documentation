# Usage

## Install
```
pip install -r requirements.txt
npm install 
npm install -g sass
```

## Build & Serve

Build static site
`mkdocs build`

Start local server 
`mkdocs serve`

> [!TIP]
> If you encounter cairo linking issue, you can try the [workaround solution](https://github.com/Kozea/cairocffi/issues/28#issuecomment-1173455046).


## Landing page
If you are updating the CSS styles for the `landing page`, it is generated from the root `sass`:
`sass --watch sass/home.sass docs/styles/home.css`

## Other customizations

[Extra css](https://www.mkdocs.org/user-guide/customizing-your-theme/) for documentation pages can be added here:

```
/docs/styles/extra.css
```

Overrides, such as the custom landing page, are stored in the following location:

```
/material/overrides

```

## Configuration options
Configuration options are set in the `mkdocs.yml` file. 
- [MKDocs Configuration](https://www.mkdocs.org/user-guide/configuration/)
- 

## Adding new feature pages
1. To add new features, create a Markdown file in `docs/docs/features` and added a title, e.g.:

```
---
title: Histogram Diff
---
```

2. Add the page to the `nav` block in `mkdocs,yml`:

```
nav:
  - index.md
  ...
  - Features:
    - docs/features/histogram-diff.md
    ...
```

3. Add images to `docs/assets/images/features`


## Customize the site
https://squidfunk.github.io/mkdocs-material/setup/


# Images

Please use [pngquant](https://pngquant.org/) to optimize any PNG files:

```
pngquant --force --quality=100 --strip --skip-if-larger --verbose <filename>.png
```

Try to keep animated gifs no bigger than around 1MB. Only use multiple large gifs on one page if absolutely neccessary. 




sasfsdfsdf