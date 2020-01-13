## How to Setup: Dynamic handlebars html pdf to create dynamic content html to pdf.

#### Installation

```
npm install dynamic-html-pdf --save

```
#### Create template.html

```
<html>
    <head>
        Dynamic HTML to PDF
    </head>
    <body>
        <h1>Hi {{users.0.name}}</h1>
        <div>
            template
            {{#ifCond 'v1' 'v2'}}
                {{v1}} is equals to {{v2}}
            {{else}}
                Variables are not similar
            {{/ifCond}}
        </div>
    </body>
</html>

```
#### Feel free to use handlebar syntax: [Handlebar builtin helpers](http://handlebarsjs.com/builtin_helpers.html)

For example:
```
<ul>
  {{#each users}}
    <li>Name: {{this.name}}</li>
    <li>Age: {{this.age}}</li>
    <li>DOB: {{this.dob}}</li>
  {{/each}}
</ul>
```

#### [Custom handlebar helpers](https://handlebarsjs.com/block_helpers.html)
For example:
Register helper inside js file:
```
// Custom If condition inside handlebar(JS file)
var pdf = require('dynamic-html-pdf');
pdf.registerHelper('ifCond', function (v1, v2, options) {
    if (v1 === v2) {
        return options.fn(this);
    }
    return options.inverse(this);
})
```

Utilize registered helper inside handlebar template:
```
{{#ifCond v1 v2}}
    {{v1}} is equals to {{v2}}
{{else}}
    Variables are not similar
{{/ifCond}}
```


#### How to use Dynamic HTML to PDF

```
var fs = require('fs');
var pdf = require('dynamic-html-pdf');
var html = fs.readFileSync('template.html', 'utf8');

// Custom handlebar helper
pdf.registerHelper('ifCond', function (v1, v2, options) {
    if (v1 === v2) {
        return options.fn(this);
    }
    return options.inverse(this);
})

var options = {
    format: "A3",
    orientation: "portrait",
    border: "10mm"
};

var users = [
    {
        name: 'aaa',
        age: 24,
        dob: '1/1/1991'
    },
    {
        name: 'bbb',
        age: 25,
        dob: '1/1/1995'
    },
    {
        name: 'ccc',
        age: 24,
        dob: '1/1/1994'
    }
];

var document = {
    type: 'buffer',     // 'file' or 'buffer'
    template: html,
    context: {
        users: users
    },
    path: "./output.pdf"    // it is not required if type is buffer
};

pdf.create(document, options)
    .then(res => {
        console.log(res)
    })
    .catch(error => {
        console.error(error)
    });
```
