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
 - QR Code
 - Image
 - Horizontal line
 - Table

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
    barCode: {
        width: 1,
        height: 30,
        text: "INC0000123",
        marginUnderText: 5,
        fontSizeUnderText: 10
    }
}
```

## QR code component
The PDF template supports the QR code visualisation. The following attributes are part of the QR code component:
Basic parameters:
- width: With of the generated image. Default value: 200
- height: Height of the generated image. Default value: 200
- type: constant value: SVG
- data: This is the content of the QR code!

Appearance parameters
- backgroundOptions.color: Main background color in hexadecimal format
- dotsOptions.type: There can be three types:
  - square
  - dots
  - rounded
- dotsOptions.color: Color of dots in hexadecimal format
- cornersSquareOptions.type: There can be multiple styles of the corner squares:
  - none (empty value)
  - square
  - dot
- cornersSquareOptions.color Color of corner squares in hexadecimal format
- cornersDotOptions.type: There can be multiple styles of the corner dots:
  - none (empty value)
  - square
  - dot
- cornersDotOptions.color Color of corner dots in hexadecimal format

A simple QR code definition can be found below
``` JS
{
    qrCode2: {
	options: {
	    "width": 200,
	    "height": 200,
	    "type": "svg",
	    "data": "Test content!!!",
	    "imageSysId": "",
	    "imageOptions": {
	    },
	    "qrOptions": {
		"typeNumber": "0",
		"mode": "Byte",
		"errorCorrectionLevel": "Q"
	    },
	    "backgroundOptions": {
		"color": "#ffffff"
	    },
	    "dotsOptions": {
		"type": "square",
		"color": "#000000",
		"gradient": null
	    },
	    "cornersSquareOptions": {
		"type": "",
		"color": "#000000"
	    },
	    "cornersDotOptions": {
		"type": "",
		"color": "#000000"
	    }
	}
    }
}
```

## Image component
Images also can be added to the PDF. There are several modes how an image can be added to the PDF document
- Based on ServiceNow attachment
- Using image URL
- Using Base64 encoded image

ServiceNow attachment
In this case the image has to be added to the sys_attachment table first and then, the sys_id of the record has to be used in the template
``` JS
{
	image:
	{
		sys_id: "a0a54ccd7715b110f40ab95d0eff3592"
	}
}
```

Using image URL
When the URL is used only images URLs can be added which are uploaded to the ServiceNow system
``` JS
{
	image:
	{
		url: "image.png"
	}
}
```

Using Base64 encoded image

Base64 encoded images are also supported. The following JSON structure has to be used in order to use Base64 encoded images.

``` JS
{
	image: {
  		b64: {
			content_type: "image/png",
			data: "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"
		}
	}
}
```

## Horizontal line component
A simple horizontal line can be used like in HTML. using the following JSON structure

``` JS
{
    hr: {}
}
```

The base horizontal line doesn't look good. It is highly recommended adding some style to the component, like the below one:

``` JS
{
	hr: {},
	style: [
	    "color: black;",
	    "background-color: black;",
	    "height: 1px;",
	    "border: 0px;"
	]
},
```

The horizontal line definition above looks much better. It is a simple black line with 1 px height. This is a simple CSS definition.

## Table component
The table component is the most complex one. There are several possibilities how a table can be defined into the PDF. It is important, everything is based on the main HTML component. The table component uses the same concept.

A simple table definition can be seen below, with three columns and two rows. The table has header, but doesn't have border:
``` JS
{
    table: {
        header: [
            {
                text: "Header 1"
            },
            {
                text: "Header 2"
            },
            {
                text: "Header 3"
            }
        ],
        body: [
            [
                {
                    text: "Row 1 Cell 1"
                },
                {
                    text: "Row 1 Cell 2"
                },
                {
                    text: "Row 1 Cell 3"
                }
            ],
            [
                {
                    text: "Row 2 Cell 1"
                },
                {
                    text: "Row 2 Cell 2"
                },
                {
                    text: "Row 2 Cell 3"
                }
            ]
        ]
    }
}
```

In the next example the table does not have headerm, but it has five rows and three columns. The table widht is fix 500 px. The *tableStyle* property contains the table level styles.

``` JS
{
table:{
    header:[
    ],
    body:[
	[
	    {
		text: "Row 1 Cell 1"
	    },
	    {
		text: "Row 1 Cell 2"
	    },
	    {
		text: "Row 1 Cell 3"
	    }                            
	],
	[
	    {
		text: "Row 2 Cell 1"
	    },
	    {
		text: "Row 2 Cell 2"
	    },
	    {
		text: "Row 2 Cell 3"
	    }                            
	],
	[
	    {
		text: "Row 3 Cell 1"
	    },
	    {
		text: "Row 3 Cell 2"
	    },
	    {
		text: "Row 3 Cell 3"
	    }                            
	],
	[
	    {
		text: "Row 4 Cell 1"
	    },
	    {
		text: "Row 4 Cell 2"
	    },
	    {
		text: "Row 4 Cell 3"
	    }                            
	],
	[
	    {
		text: "Row 5 Cell 1"
	    },
	    {
		text: "Row 5 Cell 2"
	    },
	    {
		text: "Row 5 Cell 3"
	    }                            
	]
    ]
},
tableStyle: [
    "border-collapse: collapse;",
    "width: 500px"
    ]
}
```

The next example contains an additional style. Each row has top and bottom border, with a grey color. This style is defined into the global style property of the template.


``` JS
content: [
{
table:{
    header:[
    ],
    body:[
	[
	    {
		text: "Row 1 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 1 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 1 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 2 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 2 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 2 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 3 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 3 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 3 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 4 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 4 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 4 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 5 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 5 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 5 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	]
    ]
},
tableStyle: [
    "border-collapse: collapse;",
    "width: 500px"
    ]
}
],
styleClasses: {
    borderBody: {
	"border-top": "1px solid lightgrey",
	"border-bottom": "1px solid lightgrey"
    },
}
```

In the following example, the style is a bit more complex. The upper border of the first row is slightly thicker, the lower border of the last row is also thicker than the borders of the other rows.

``` JS
content: [
{
table:{
    header:[
    ],
    body:[
	[
	    {
		text: "Row 1 Cell 1",
		bodyCellStyleClass: "borderBodyFirst"
	    },
	    {
		text: "Row 1 Cell 2",
		bodyCellStyleClass: "borderBodyFirst"
	    },
	    {
		text: "Row 1 Cell 3",
		bodyCellStyleClass: "borderBodyFirst"
	    }                            
	],
	[
	    {
		text: "Row 2 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 2 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 2 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 3 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 3 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 3 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 4 Cell 1",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 4 Cell 2",
		bodyCellStyleClass: "borderBody"
	    },
	    {
		text: "Row 4 Cell 3",
		bodyCellStyleClass: "borderBody"
	    }                            
	],
	[
	    {
		text: "Row 5 Cell 1",
		bodyCellStyleClass: "borderBodyLast"
	    },
	    {
		text: "Row 5 Cell 2",
		bodyCellStyleClass: "borderBodyLast"
	    },
	    {
		text: "Row 5 Cell 3",
		bodyCellStyleClass: "borderBodyLast"
	    }                            
	]
    ]
},
tableStyle: [
    "border-collapse: collapse;",
    "width: 500px"
    ]
}
],
styleClasses: {
    borderBodyFirst: {
	"border-top": "2px solid black"
    },

    borderBody: {
	"border-top": "1px solid lightgrey"
    },

    borderBodyLast: {
	"border-top": "1px solid lightgrey",
	"border-bottom": "2px solid black"
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
