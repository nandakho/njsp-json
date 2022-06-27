# njsp-json

Based on Node-Jasper project,
simplified to generate report from JSON data only.

## Install

Install via npm:

```
npm install --save njsp-json
```

To use it inside your project just do:

```
var jasper = require('njsp-json')(options);
```

Where _options_ is an object with the following signature:

```
options: {
	path: , //Path to jasperreports-x.x.x directory (from jasperreports-x.x.x-project.tar.gz)
	reports: {
 		// Report Definition
 		name: { //Report's name
 			jasper: , //Path to jasper file,
 			jrxml: //Path to jrxml file
 		}
 	},
	tmpPath: '/tmp', // Path to a folder for storing compiled report files, default is used if not provided
 }
 ```

## API

* **java**

	Instance of *node-java* that we are currently running.

* **add(name, report)**

  Add a new _report_ definition identified by _name_.

  In report definition one of _jasper_ or _jrxml_ must be present.

* **html(report)**

  Alias for _export(report, 'html')_

* **doc(report)**

  Alias for _export(report, 'doc')_

* **xls(report)**

  Alias for _export(report, 'xls')_

* **ppt(report)**

  Alias for _export(report, 'ppt')_

* **pdf(report)**

  Alias for _export(report, 'pdf')_

* **export(report, format)**

  Returns the compiled _report_ in the specified _format_.

  report is
  * An object that represents reports, data and properties to override for this specific method call.

    ```
    {
      report: , //name, definition or an array with any combination of both
      data: {}, //Data to be applied to the report. If there is an array of reports, data will be applied to each.
      override: {} //properties of report to override for this specific method call.
      dataset: {} //an object to be JSON serialized and passed to the Report as fields instead of parameters (see the example for more info)
	  query: '' // string to pass to jasperreports to query on the dataset
 	}
 	```
  * An array with any combination of the three posibilities described before.

  * A function returning any combination of the four posibilities described before.

## Example

```
var express = require('express'),
	app = express(),
	jasper = require('njsp-jasper')({
		path: '../jasper',
		reports: {
			hw: {
				jasper: 'reports/helloWorld.jasper'
			}
		}
	});

	app.get('/pdf', function(req, res, next) {
		var report = {
			report: 'hw',
			data: {
				id: parseInt(req.query.id, 10)
				secondaryDataset: jasper.toJsonDataSource({
					data: {...}
				},'data')
			}
			dataset: [{...},{...}] //main dataset
		};
		var pdf = jasper.pdf(report);
		res.set({
			'Content-type': 'application/pdf',
			'Content-Length': pdf.length
		});
		res.send(pdf);
	});

	app.listen(3000);
```

That's It!.
