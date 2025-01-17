Description: The following definition list incident records. This is a complex PDF document with header and footer. In case of multiple pages, each page contains the header of table ("repeat-header": "yes")

Template object:
var pdfTemplateProcessor = function(tmplParams) {

	var dataListParams = {
		"pageTitle": "Exported Incidents",
		"table": "incident",
		"query": "active=true^caller_idDYNAMIC90d1921e5f510100a9ad2572f2b477fe",
		"orderField": "number",
		"fields": [
			"number",
			"short_description",
			"state",
			"opened_by",
			"sys_created_on",
			"caller_id",
			"sys_updated_on"
		]
	};

    function addExtraSpaceToLongWords(str) {
		if (str) {
			var maxNumberOfChars = 28;
			var retval = "";
			var strArray = str.split(" ");
			for (var i = 0; i < strArray.length; i++) {
				if (strArray[i].length > maxNumberOfChars) {
					retval += addSpaceAfterNthChars(strArray[i], maxNumberOfChars);
				}
				else {
					retval += strArray[i] + " ";
				}
			}
			return retval.trim();
		}
		return "";
    }

    function addSpaceAfterNthChars(str, numberofChar) {

        var result = '';
        for(var i = 0; i < str.length; i++) {
            result += str[i];
            if((i + 1) % numberofChar === 0) {
                result += ' ';
            }
        }
        return result;
    }

	var docDefinition = {
		pageSize: function() {
            var fLength = dataListParams.fields.length < 1 ? 1 : dataListParams.fields.length;
			return fLength > 20 ? '1188mm 841mm' : (fLength > 12 ? '841mm 594mm' : (fLength > 8 ? '594mm 420mm' : (fLength > 5 ? '420mm 297mm' : '297mm 210mm')));
        }(), /*'A4',*/
		pageOrientation: '',
		mainFont: "TeleNeo",
        mainFontSize: "12px",
        pageMargins: [72, 20, 40, 20],
		content: [
            {
                table: function() {
                    var _header = [],
                        _body = [];

                    var gr = new GlideRecord(dataListParams.table);
                    gr.addEncodedQuery(dataListParams.query);
                    if (dataListParams.orderField) {
                        gr.orderBy(dataListParams.orderField);
                    }
                    gr.query();

                    var columns = dataListParams.fields;
                    //var colMaxLength = {};

                    var lineNumber = 1;
                    var rowData = [];
                    while (gr.next()) {
                        rowData = [];
                        for (var i = 0; i < columns.length; i++) {
                            var currentCellContent = addExtraSpaceToLongWords(gr.getDisplayValue(columns[i] + ""));
                            rowData.push({
                                text: currentCellContent,                                
                                //bodyCellStyleClass: (lineNumber % 2 == 0) ? "textStyle" : "textStyle greyRow",                                
                                bodyCellStyleClass: "tableBodyCell"
                            });
                        }
                        _body.push(rowData);
                        lineNumber++;
                    }

                    if (_body.length == 0) {
                        rowData = [{
                                text: gs.getMessage("There is not result"),
                                bodyCellStyleClass: "tableBodyCell",
                                colspan: columns.length,
                                style: [
                                    "text-align: center",
                                    "padding-top: 10px",
                                    "padding-bottom: 10px"
                                ]
                            }];                        
                        _body.push(rowData);
                    }

                    for (var j = 0; j < columns.length; j++) {
                        _header.push({
                            text: gr[columns[j] + ""].getLabel()
                        });
                    }

                    return {
                        header: _header,
                        body: _body
                    };
                }(),                
                headerRowStyleClass: "tableHeaderRow",
                headerCellStyleClass: "tableHeaderCell",
                
                styleClass: "tableStyle"
            }
		],
        header: 
        {
            table: {
                header: [],
                body: [
                    [{
                        text: dataListParams.pageTitle,
                        style: [
                            "font-size: 14px",
                            "text-align: left",
                            "color: white",
                            "padding-left: 10px"
                        ]
                    },
                    {
                        text: [
                            {
                                text: "Exported By ",
                                style: [
                                    "color: #F399C7"
                                ]
                            },
                            {
                                text: function() {
                                    return gs.getUserDisplayName();
                                }()
                            },
                            {
                                text: " / "
                            },
                            {
                                text: function() {
                                    var currentdate = new GlideDate();
                                    return currentdate.getByFormat("MM-dd-yyyy");
                                }(),
                                style: [                                     
                                    "padding-right: 10px"
                                ]
                            }
                        ],
                        style: [
                            "font-size: 14px",
                            "text-align: right",
                            "color: white"
                        ]
                    }
                    ]
                ]
            },                
            styleClass: "headerTableStyle"
        },
		footer: {
            table: {
                header: [],
                body: [
                    [
                        {
                            text: [{
                                image: {
                                    url: "x_tsigh_csc_case.ts-logo.png"
                                },
                                style: [
                                    "width: 80"
                                ]
                            }],
                            bodyCellStyleClass: "footerTableCellStyle"
                        }, {
                            text: gs.getMessage("csc.portal.title"),
                            bodyCellStyleClass: "footerTableCellStyle2",
                            style: [
                                "padding-left: 10px",
                                "border-left: 1px solid #D9D9D9"
                            ]
                        }
                    ,{
                        text: [
                            {text: "", styleClass: "currentPageNumber"},
                            {text: " of "},
                            {text: "", styleClass: "allPageNumber"}
                        ],
                        style: [
                            "font-size: 12px",
                            "text-align: right", 
                            "align: center"                           
                        ]
                    }]
                ]
            },                
            styleClass: "footerTableStyle"	
		},		
        styleClasses: {
            tableHeaderRow: {
                backgroundColor: "rgb(255, 255, 255)",
                "font-size": "12px",
                color: "black",
                bold: true
            },
            tableBodyCell: {
                "border-top": "0px",
                "border-left": "0px",
                "border-right": "0px",
                "border-bottom": "1px solid grey",
                "page-break-inside": "avoid", 
                "page-break-after": "auto"
            },
            tableHeaderCell: {
                "border-top": "0px",
                "border-left": "0px",
                "border-right": "0px",
                "border-bottom": "2px solid black",
                "text-align": "left",
                "min-width": "fit-content"
            },
            tableStyle: {
                width: "100%",
                "border-collapse": "collapse",
                "table-layout": "fixed",
                "repeat-header": "yes"
            },
            textStyle: {
                "font-size": "12px"
            },
            headerTableStyle: {
                width: "100%",
                "border-collapse": "collapse",
                "background-color": "#E20074",
                height: "30px"
            },
            footerTableStyle: {
                width: "100%",
                "border-collapse": "collapse",                
                height: "30px"
            },
            footerTableCellStyle: {
                "width": "90px",
                "text-align": "left",
                "padding-bottom": "5px"
            },
            footerTableCellStyle2: {
                "text-align": "left",
                "padding-bottom": "10px"
            },
		}  
	};
	return docDefinition;
};
