# Web Form Parser for PHP

**Description:** PHP Web Form Form Parser Using XPath

This PHP class will parse a webform, and extract it's various settings needed to recreate a new web form that includes all necessary inputs, labels, actions and methods.

This class was created for an app that needed to allow a user to input their web form from multiple sources (mailchimp, aweber etc.) and have that form be implemented into a web page programmatically and without the need to connect to multiple APIs.

## USAGE

Include the class in your php app:

```
require_once('form-parser.php');
```

Send a web form to the constructor function of the class in HTML format:

```
$parsed = new form_parser("<form action='action.php' method='POST'>...</form>");
```

You will now have an array of elements and attributes from the form:

(example in JSON format for readability)


```
{
  forms: [ // array of forms contained in the string
    {
      label: string,              // label text (if found)
      target: string,             // target attribute from form element
      method: string,             // method attribute from form element
      action: string,             // action attribute from form element
      stylesheets: array(),       // array of stylesheets from the HTML code (in HTML format)
      scripts: array(),           // array of scripts from the HTML code (in HTML format)
      formElements: {             // array of form elements
        [id]: {                   // [id]: id attribute if available, otherwise element index
          value: string,          // value attribute of element (if available)
          class: string,          // class attribute of element (if available)
          name: string,           // name attribute of element
          required: 'required,    // required attribute of element (if available - blank if not)
          type: string,           // type attribute or tag name
          options: array()        // if type == 'select' then array of options in the select tag
        }
        ...
      }
    }
    ...
  ]
}
```

You can also render the form elements back to HTML. The below function will output only the form elements (not the form tag or the script/style tags) with their respective attributes.

```
echo $parsed->render_elements();
```

If you want to render selective arguments, you can pass an array of element ids to the same function:

```
$args = array(
  'FNAME' => true,
  'LNAME' => true
);

echo $parsed->render_elements($args);
```

This will render only the two form elements FNAME and LNAME, and skip the rest.
