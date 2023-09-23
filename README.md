# formio-camunda

We have integrated [Form.io vanilla js library](http://formio.github.io/formio.js/) for use as [external forms](https://github.com/camunda/camunda-bpm-examples/tree/master/sdk-js/browser-forms-angular) in Camunda BPM 7 or lower. 

I wouldn’t call it specifically elegant, but the approach works as follows:

1. Standalone Formio builder is used to generate a JSON file
1. We have a stub form-starter html that loads all js dependencies
1. This stub parses URL (when external form is called from tasklist) and gets taskId
1. Stub sends several requests to camunda REST API to get info on a task, name of a JSON file to load (task input parameters) and variables
1. Stub loads JSON and visualises it as a form
1. Stub may pre-populate form if submission data is provided
1. Stub is also capable to load over-ride values for specific submission fields
1. On submit stub is calling camunda REST API to send submission in a JSON format and stores it as a variable

## Installation & Caveats

Clone example from here:

```bash
git clone https://github.com/julrichkieffer/formio-camunda.git
```

Then run this in the root directory

```bash
mvn clean package
```

### Caveats
As far as I remember it does not have external dependencies and comes with a custom version of the formio (as we use Box integration of the File component).

It works for me on a standard distribution of 7.9 with Tomcat.

It supports only basic controls and forms (no support for formio workflows).

Fancy controls like Signature will crash form / camunda on submission.

JSON files should contain escaped json objects (with quotes). You can play with toy JSON examples from here: http://formio.github.io/formio.js/app/examples/json.html

## Task Configuration

We use Form Builder to produce form definition (schema) in JSON and later load it via the [formio renderer]((http://formio.github.io/formio.js/).

Generate an html file that will be treated as an external form, and refer to it depending on your deployment type. In our case we deploy via war files, so the reference is as follows:

![](https://forum.camunda.io/uploads/default/original/2X/3/380ecab59959b8fcf3d557237222eabf63e468f0.png)

And the stub also reads input parameters:
- json: name of the Formio JSON file with form definition
- load_submission: variable to prepopulate form
- save_submission: variable to store form contents on submit

![](https://forum.camunda.io/uploads/default/original/2X/3/346455f52bc176e5b45e6ffc24941823b29fed04.png)
