# koa-error

  Error response middleware for koa supporting:

- text
- json
- html

# 源码分析

利用try-catch，配合koa的洋葱模型，在最外层使用该中间件，即可达到异常捕获效果。

通过ctx.accepts类型的不同，采用不同的处理方法。

## Installation

```js
$ npm install koa-error
```

## Options

 - `template` path to template written with your template engine
 - `engine` template engine name passed to [consolidate](https://github.com/tj/consolidate.js)
 - `cache` cached compiled functions, default: `NODE_ENV != 'development'`
 - `env` force a NODE_ENV, default: `development`
 - `accepts` mimetypes passed to [ctx.accepts](https://github.com/koajs/koa/blob/master/docs/api/request.md#requestacceptstypes), default: `[ 'html', 'text', 'json' ]`

## Custom templates

  By using the `template` option you can override the bland default template,
  with the following available local variables:

  - `env`
  - `ctx`
  - `request`
  - `response`
  - `error`
  - `stack`
  - `status`
  - `code`

Here are some examples:

### Pug (formerly jade)

```js
app.use(error({
  engine: 'pug',
  template: __dirname + '/error.pug'
}));
```

```jade
doctype html
html
  head
    title= 'Error - ' + status
  body
    #error
      h1 Error
      p Looks like something broke!
      if env == 'development'
        h2 Message:
        pre: code= error
        h2 Stack:
        pre: code= stack
```

### Nunjucks

```js
app.use(error({
  engine: 'nunjucks',
  template: __dirname + '/error.njk'
}));
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Error - {{status}}</title>
  </head>
  <body>
    <div id="error">
      <h1>Error</h1>
    <p>Looks like something broke!</p>
    {% if env == 'development' %}
      <h2>Message:</h2>
      <pre>
        <code>
{{error}}
        </code>
      </pre>
      <h2>Stack:</h2>
      <pre>
        <code>
{{stack}}
        </code>
      </pre>
    {% endif %}
    </div>
  </body>
</html>
```

## License

  MIT
