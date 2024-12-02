# PDF Template components

The iText Java class library generates the PDF from an HTML template. The template editor hides this code from the developer and gives a kind of Javascript class structure to implement a template, but it is important, when somebody is working on a template, needs to follow the HTML+CSS mindset. 


# Component list

There are several components, which are supported by the application:

 - Text
 - New line
 - New page
 - HTML Content 
 - SVG Content
 - Barcode

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
```

## Barcode component
The PDF template supports the barcode visualisation. The following attributes are part of the barcode:

- width: Most of the time the value is 1
- height: This property specifies the height of the barcode. Default value: 30
- text: This property is the content of the barcode
- marginUnderText: This property defines the distance between the barcode the the text below
- fontSizeUnderText: This property defines the font size of the text below the barcode in pixel.

Example:

``` JS

{
    barcode: {
        width: 1,
        height: 30,
        text: "INC0000123",
        marginUnderText: 5,
        fontSizeUnderText: 10
    }
}
```


# Styling

The whole solution is HTML based, so CSS components can be used for defining styles.
Each component can have its own style definition (like an HTML component) but style class also can be used.

## Text component style

The direct style definition has to be added to the JSON element, called "style". This is an array, which contains a list of CSS properties.

### Direct style definition

``` JS
 {
     text: "Hello World!",
     style: [
         "color: red",
         "font-weight: bold"
     ]
 }
```
OR

``` JS
 {
     text: "Hello World!",
     style: [
         "width: 100%",
         "text-align: right",
     ]
 }
```

### Style definition using style class

In this case the style definition is part of the styleClasses property of pdfTemplateProcessor document template. This is an object with key value pairs.

Example:

``` JS
 styleClasses: {
     class1: {
         "width": "100%",
         "text-align": "right"
     },
     class2: {
         "color": "red"
     }
 }
```

Example with text component:


``` JS
 content: [
     {
         text: "Hello World!",
         styleClass: "class1"
     }
 ],
 styleClasses: {
     class1: {
         "width": "100%",
         "text-align": "right"
     },
     class2: {
         "color": "red"
     }
 }
```

The example below represents how multiple style classes can be added to a text component.

``` JS
 content: [
     {
         text: "Hello World!",
         styleClass: "class1 class2"
     }
 ],
 styleClasses: {
     class1: {
         "width": "100%",
         "text-align": "right"
     },
     class2: {
         "color": "red"
     }
 }
```

## HTML component style

Example:

``` JS
{
    htmlContent: "<b>Hello World</b><br><i>Sample text</i>",
    style: [
        "width: 50%",
        "border: 1px solid black"
    ]
}
```

# Header and Footer

The solution supports to add header and / or footer to the template.

The header and footer attribute of the template is a simple object, so only one component can be added, but the components can be nested, so the header and footer can also be a complex content. Examples:

## Header

``` JS
header: {
     text: "Document header",
     style: [
         "width: 100%",
         "text-align: center",
         "font-style: italic"
     ]
 }
```

``` JS
header: {
    text: [
        {
            htmlContent: "<b>Hello World</b><br><i>Sample text</i>",
            style: [
                "width: 50%",
                "border: 1px solid black"
            ]
        }
    ]           
}
```

## Footer

``` JS
footer: {
        text: [{
                text: "Footer text:"
            },
            {
                text: "",
                styleClass: "currentPageNumber"
            },
            {
                text: "/"
            },
            {
                text: "",
                styleClass: "allPageNumber"
            }
        ]
    }
```

## Special elements
### Page number
The page number definition is also supported by the application. There are two special styleClass elements, which have to be used:

- **currentPageNumber**: This property shows the number of current page
- **allPageNumber**: This property shows the number of all pages of PDF document
