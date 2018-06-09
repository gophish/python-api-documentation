# Landing Pages

Landing pages contain the HTML that is rendered when a target clicks on a Gophish phishing link.

The pages endpoint allows you to create, view, and manage Gophish landing pages.

## Table of Contents

* [Quick Example](landing-pages.md#quick-example)
* [Models](landing-pages.md#models)
* [Methods](landing-pages.md#methods)
* [Examples](landing-pages.md#examples)

## Quick Example

This example shows how to retrieve the name of every page in Gophish.

```python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for page in api.pages.get():
    print page.name
```

### Models

#### gophish.models.Page

A page contains one or more `models.User` objects. The page name must be unique.

**Attributes**

* `id` \(int\) The page ID
* `html` \(str\) The page HTML
* `name` \(str\) The page name
* `modified_date` \(optional: datetime.datetime\) The scheduled time for page launch
* `capture_credentials` \(bool default:False\) Whether or not the landing page should capture credentials
* `capture_passwords` \(bool default:False\) Whether or not the landing page should capture passwords
* `redirect_url` \(str\) The URL to redirect targets to after they submit data

**Methods**

* `__init__(self, **kwargs)` - Returns a new Landing Page

Example:

```python
from gophish.models import *

page = Page(name='Test Page', 
   html="<html><body>Click <a href="{{.URL}}">here</a></body></html>)
```

### Methods

#### gophish.api.pages.get\(page\_id=None\)

Gets the details for one or more landing pages. To get a particular page, set the ID to the page ID.

If the `page_id` is not set, all landing pages owned by the current user will be returned.

**Returns**

* If the `page` is set: `models.Page`
* If `page_id` is `None`: `list(models.Page)`

#### gophish.api.pages.post\(page\)

Creates a new landing page. This endpoint requires you to submit a `gophish.models.Page` object.

**Returns**

The `gophish.models.Page` object that was created.

#### gophish.api.pages.put\(page\)

Edits an existing landing page. This endpoint requires you to submit an existing `gophish.models.Page` object with its `id` attribute set correctly.

**Returns**

The `gophish.models.Page` object that was edited.

#### gophish.api.pages.delete\(page\_id\)

Deletes the page specified by `page_id`.

**Returns**

A `gophish.models.Status` message.

### Examples

Here are some examples to show how to use the API.

All of these examples assume the following setup:

```python
from gophish import Gophish
from gophish.models import *

api_key = 'API_KEY'
api = Gophish(api_key)
```

#### Get All Landing Pages

```python
pages = api.pages.get()
```

#### Get Single Landing Page

```python
page = api.pages.get(page_id=1)
```

#### Create New Landing Page

```python
page = Page(name='Test Page', 
   html="<html><body>Click <a href="{{.URL}}">here</a></body></html>)

page = api.pages.post(page)
print page.id
```

