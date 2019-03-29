![Pill logo](docs/cover.png)

Pill adds dynamic content loading to static sites and makes content loading
smooth for users. It's pretty small only _1 KiB_ minified and gziped. It fit's perfectly
for static sites with WebComponents.

Pill development started with Andrey Sitnik's [@ai](https://github.com/ai) [twit](https://twitter.com/sitnikcode/status/1109626507331338240).

How pill works. It:

1. Intercepts navigation attempts.
2. Loads requested url using `fetch`.
3. Grabs content from received HTML.
4. Replaces current page content.

## Install

Include script from unpkg.com:
```html
<script src="https://unpkg.com/pill@1/dist/pill.min.js"></script>
```

> Don't forget about security. Add [subresource integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) (SRI) checksum
> from [checksum.txt](unpkg.com/pill@1/dist/checksum.txt).

or via npm

```
npm i pill
```

## Usage

1. Inject pill.js with `<script>`
2. Create div element and give it meaningful id.
3. Create loading indicator.
4. Initialize pill:
  ```javascript
  pill('#content')
  ```

### Complete example

```html
<html>
  <head>
    <title>Home</title>
    <script src="https://unpkg.com/pill@1/dist/pill.min.js"></script>
    <style>
      /* global styles */
      #indicator {
        position: fixed;
        top: 0;
        right: 0;
        display: none;
      }
    </style>
  </head>
  <body>
    <div id="indicator">Loading...</div>
    <div id="content">
      <style>/* page styles */</style>
      
      <!-- page content here -->
    </div>
    <script>
      const loadingIndicator = document.querySelector('#indicator')

      pill('#content', {
        onLoading() {
          // Show loading indicator
          indicator.style.display = 'initial'
        },
        onReady() {
          // Hide loading indicator
          indicator.style.display = 'none'
        }
      })
    </script>
  </body>
</html>
```

Each document of the site should surround `#content` element with the same HTML.
All page-related content should be located inside `#content`. It could be styles, scripts, etc.

## API

### `pill()`
```
(selector:string, options:Options) -> void
```

Initialize pill. Start listening for navigation attempts and history state changes. Puts loaded
content into `selector` element.

### `Options.onLoading()`
```
(page:Page) -> void
```
Handle loading start.

### `Options.onReady()`
```
(page:Page) -> void
```
Handle loading finish.

### `Options.resolve()`
```
(url:URL) -> URL
```
Transform requested url before fetching,

### `Options.fromError()`
```
(error:Error) -> {title, content}
```
Use it to display notification when something went wrong.
If an error was thrown while handling request. You still able
to render content using method `fromError`

### `Options.shouldServe()`
```
(url) -> boolean
```
Determine wither URL could be served by Pill. If returns `false` than link
will be served by browser.

### `shouldReload()`
```
(page) => boolean
```

Determine wether page should be reloaded.

## License

MIT © [Rumkin](https://rumk.in)
