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
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
        content: [
	  {
	    text: 'Hello World',
	    styleClass: "specialStyle"
	  },
	],
        header: {
        },
        footer: {
        },
        styleClasses: {
	  specialStyle: {
	    color: "red"
	  }
	}
    };
    return docDefinition;
};
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
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
        content: [
	{
	table: {
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
        header: {
        },
        footer: {
        },
        styleClasses: {
	    borderBody: {
		"border-top": "1px solid lightgrey",
		"border-bottom": "1px solid lightgrey"
	    },
	}
    };
    return docDefinition;
};
```

In the following example, the style is a bit more complex. The upper border of the first row is slightly thicker, the lower border of the last row is also thicker than the borders of the other rows.

``` JS
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
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
        header: {
        },
        footer: {
        },
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
    };
    return docDefinition;
};
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
