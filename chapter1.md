# Campaigns

The campaigns endpoint allows you to create, view, and manage Gophish campaigns.

## Table of Contents

* [Quick Example](#quick-example)
* [Models](#models)
* [Methods](#methods)

## Quick Example

This example shows how to retrieve the names of every campaign launched in Gophish.

``` python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for campaign in api.campaigns.get():
    print campaign.name
```

### Models

#### gophish.models.Result

##### Attributes

 * `id` (int) The result ID
 * `first_name` (str) The first name
 * `last_name` (str) The last name
 * `email` (str) The email address
 * `position` (str) The position (job role)
 * `ip` (str) The last seen IP address
 * `latitude` (float) The latitude of the `ip`
 * `longitude` (float) The longitude of the `ip`
 
##### Methods

 * `__init__(self, **kwargs)` - Returns a new Result
 
 Example:
 
``` python
 result = Result(
     first_name='Foo', last_name='Bar',
     position='Tester', email='foo.bar@example.com')
```
 
#### gophish.models.Campaign

For each of the attributes in the campaign (groups, template, page, etc.), Gophish **only cares about the `name`**.

This means that you don't have to fetch the resources you want to use. You can simply create new ones with the correct name for the campaign (see the example below).

##### Attributes

 * `id` (int) The result ID
 * `results` (list(models.Result)) The campaign results
 * `timeline` (list(models.TimelineEntry)) The timeline entries
 * `name` (str) The campaign name
 * `created_date` (optional: datetime.datetime) The campaign creation date
 * `launch_date` (optional: datetime.datetime) The scheduled time for campaign launch
 * `template` (models.Template) The template to use in the campaign
 * `page` (models.Page) The Landing Page to use in the campaign
 * `smtp` (models.SMTP) The SMTP Profile to use when sending emails
 * `url` (str) The URL to use when constructing links in phishing emails
 
##### Methods

 * `__init__(self, **kwargs)` - Returns a new Campaign
 
 Example:
 
``` python
groups = [Group(name='Existing Group')]
page = Page(name='Existing Page')
template = Template(name='Existing Template')
profile = SMTP(name='Existing Profile')
url = 'http://phishing_server'

campaign = Campaign(
    name='Example Campaign', groups=groups, page=page,
    template=template, profile=profile)
```

### Methods

#### gophish.api.campaigns.get(campaign_id=None)

Gets the details for one or more campaigns. To get a particular campaign, set the ID to the campaign ID.

If the `campaign_id` is not set, all campaigns owned by the current user will be returned.

**Returns**

* If the `campaign_id` is set: `models.Campaign`

* If `campaign_id` is `None`: `list(models.Campaign)`

#### gophish.api.campaigns.post(campaign)

Creates and launches a new campaign. This endpoint requires you to submit a `gophish.models.Campaign` object.

**Returns**

The `gophish.models.Campaign` object that was created.

#### gophish.api.campaigns.delete(campaign_id)

Deletes the campaign specified by `campaign_id`.

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

#### Get All Campaigns

``` python
campaigns = api.campaigns.get()
```

#### Get Single Campaign

``` python
campaign = api.campaigns.get(campaign_id=1)
```

#### Create New Campaign

``` python
groups = [Group(name='Existing Group')]
page = Page(name='Existing Page')
template = Template(name='Existing Template')
profile = SMTP(name='Existing Profile')
url = 'http://phishing_server'
campaign = Campaign(
    name='Example Campaign', groups=groups, page=page,
    template=template, profile=profile)

campaign = api.campaigns.post(campaign)
print campaign.id
```

#### Get Campaign Summaries