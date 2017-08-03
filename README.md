# How to Setup: Dynamic handlebars html pdf to create dynamic content html to pdf.

```
npm install dynamic-html-pdf --save

```
template.html
```
<html>
    <head>
        Dynamic HTML to PDF
    </head>
    <body>
        <h1>Hi {{user}}</h1>
        <div>
            template
        </div>
    </body>
</html>

```

How to use Dynamic HTML to PDF

```
var fs = require('fs');
var pdf = require('dynamic-html-pdf');
var html = fs.readFileSync('template.html', 'utf8');


var options = {
    format: "A3",
    orientation: "portrait",
    border: "10mm"
};

var document = {
    template: html,
    context: {
        user: 'User'
    },
    path: "./output.pdf"
};

pdf.create(document, options)
    .then(res => {
        console.log(res)
    })
    .catch(error => {
        console.error(error)
    });
```