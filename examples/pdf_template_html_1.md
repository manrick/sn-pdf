#Template definition

```JS
var pdfTemplateProcessor = function(tmplParams) {
 
    var docDefinition = {
        pageSize: 'A4',
        pageOrientation: 'portrait',
        mainFont: "Arial",
        pageMargins: [72, 20, 40, 20],
        content: [
            {
                htmlContent: "<div>Test</div>"
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

Output:

[preview.pdf](https://github.com/user-attachments/files/17977270/preview.pdf)

