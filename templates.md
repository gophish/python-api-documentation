#Templates

Templates are the emails that are sent out by Gophish during a campaign.

The templates endpoint allows you to create, view, and manage Gophish templates.

## Table of Contents

* [Quick Example](#quick-example)
* [Models](#models)
* [Methods](#methods)
* [Examples](#examples)

## Quick Example

This example shows how to retrieve the name of every template in Gophish.

``` python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for template in api.templates.get():
    print template.name
```

### Models

#### gophish.models.Attachment

##### Attributes

* `content` (str) The base64 encoded attachment content
* `type` (str) The content type of the attachment
* `name` (str) The attachment filename

##### Methods

* `__init__(self, **kwargs)` - Returns a new Attachment

Example:

``` python
todo
```

#### gophish.models.Template

A template contains a name and the email content.

##### Attributes

* `id` (int) The template ID
* `name` (str) The template name
* `html` (str) The template HTML
* `text` (str) The template HTML
* `modified_date` (optional: datetime.datetime) The scheduled time for template launch
* `attachments` (list(models.Attachment)) The optional email attachments

##### Methods

* `__init__(self, **kwargs)` - Returns a new Template

Example:

``` python
from gophish.models import *

template = Template(name='Test Template',
html="<html><body>Click <a href="{{.URL}}">here</a></body></html>)
```

### Methods

#### gophish.api.templates.get(template_id=None)

Gets the details for one or more templates. To get a particular template, set the ID to the template ID.

If the `template_id` is not set, all templates owned by the current user will be returned.

**Returns**

* If the `template` is set: `models.Template`

* If `template_id` is `None`: `list(models.Template)`

#### gophish.api.templates.post(template)

Creates a new template. This endpoint requires you to submit a `gophish.models.Template` object.

**Returns**

The `gophish.models.Template` object that was created.

#### gophish.api.templates.put(template)

Edits an existing template. This endpoint requires you to submit an existing `gophish.models.Template` object with its `id` attribute set correctly.

**Returns**

The `gophish.models.Template` object that was edited.

#### gophish.api.templates.delete(template_id)

Deletes the template specified by `template_id`.

**Returns**

A `gophish.models.Status` message.

### Examples

Here are some examples to show how to use the API.

All of these examples assume the following setup:

``` python
from gophish import Gophish
from gophish.models import *

api_key = 'API_KEY'
api = Gophish(api_key)
```

#### Get All Templates

``` python
templates = api.templates.get()
```

#### Get Single Template

``` python
template = api.templates.get(template_id=1)
```

#### Create New Template

``` python
template = Template(name='Test Template',
html="<html><body>Click <a href="{{.URL}}">here</a></body></html>)

template = api.templates.post(template)
print template.id
```
