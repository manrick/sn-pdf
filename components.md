# PDF Template components

The iText Java class library generates the PDF from an HTML template. The template editor hides this code from the developer and gives a kind of Javascript class structure to implement a template, but it is important, when somebody is working on a template, needs to follow the HTML+CSS mindset. 


# Component list

There are several components, which are supported by the application:

 - Text
 - New line
 - New page
 - HTML Content 
 - SVG Content

# Template definition

The template itself is a Javascript class with some meta elements. The structure of object frame can be seen below:

``` JS
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
        content: [
            {
                text: "Hello World"
            }
        ],
        header: {
        },
        footer: {
        },
        styleClasses: {}
    };
    return docDefinition;
};
```

This is the main frame of the template.

**Document template attributes:**

- pageSize: more detailed information can be seen [HERE](https://developer.mozilla.org/en-US/docs/Web/CSS/@page/size)
Example:

``` JS
pageSize: 'A3'
```

- pageOrientation: It can be landscape or portrait
Example:

``` JS
pageOrientation: 'portrait'
```

- pageMargins: This is the definition of page margin based on the CSS margin definition.
Example:

``` JS
pageMargins: [72, 20, 40, 20]
```

- content: This is a Javascript array, which contains the components.
Example:

``` JS
content: [ {text: 'Hello World'} ]
```

- header: Similar like the content, but here just one component can be added.
Example:

``` JS
header: { text: "Document header" }
```

- footer: Similar like the content, but here just one component can be added.
Example:

``` JS
footer: { text: "Document footer" }
```

- styleClasses: This is a JSON object, where different styles can be defined
Example:

``` JS
content: [
  {
    text: 'Hello World',
    styleClass: "specialStyle"
  },
],
styleClasses: {
  specialStyle: {
    color: "red"
  }
}
```

# Components

## Text component
Simple text object example:

``` JS
{
  text: 'Hello World'
}
``` 

Text concatenation example:

``` JS
{
  text: [
    {
      text: 'Hello'
    },
    {
      text: 'World'
    }
  ]
}
```

## New line component
This components add a line break to the PDF document

Example:

``` JS
{
  text: "_linebreak_"
}
```

OR

``` JS
{
  text: "\n"
}
```

## New page component
This component add a new page to the PDF document

``` JS
{
  addNewPage: true
}
```

## HTML component
HTML content can be added to the PDF template

Example:

``` JS
{
  htmlContent: '<b>Hello World</b><br><i>Sample text</i>'
}
```

## SVG component
SVG content can be added to the PDF template

Example:

``` JS
{
  svgContent: '<svg height="100" width="100"><circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" /></svg>'
}

}
```
