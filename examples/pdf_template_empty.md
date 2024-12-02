#Template definition

```JS
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
        content: [
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
