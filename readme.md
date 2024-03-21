# Usage

## Install
```
pip install -r requirements.txt
npm install 
npm install -g sass
```

## Update

Build static site
`mkdocs build`

Start local server 
`mkdocs serve`

Update Sass for the landing page
`sass --watch sass/home.sass docs/styles/home.css`

> [!TIP]
> If you encounter cairo linking issue, you can try the [workaround solution](https://github.com/Kozea/cairocffi/issues/28#issuecomment-1173455046).

## Customize the site
https://squidfunk.github.io/mkdocs-material/setup/


Add Extra css to:
```
/docs/styles/extra.css
```

Landing page overides in
```
/material/overrides
```
