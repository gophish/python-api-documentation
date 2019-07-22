# Campaigns

The campaigns endpoint allows you to create, view, and manage Gophish campaigns.

## Table of Contents

* [Quick Example](campaigns.md#quick-example)
* [Models](campaigns.md#models)
* [Methods](campaigns.md#methods)
* [Examples](campaigns.md#examples)

## Quick Example

This example shows how to retrieve the names of every campaign launched in Gophish.

```python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for campaign in api.campaigns.get():
    print campaign.name
```

### Models

#### gophish.models.Result

**Attributes**

* `id` \(int\) The result ID
* `first_name` \(str\) The first name
* `last_name` \(str\) The last name
* `email` \(str\) The email address
* `position` \(str\) The position \(job role\)
* `ip` \(str\) The last seen IP address
* `latitude` \(float\) The latitude of the `ip`
* `longitude` \(float\) The longitude of the `ip`

**Methods**

* `__init__(self, **kwargs)` - Returns a new Result

  Example:

```python
 result = Result(
     first_name='Foo', last_name='Bar',
     position='Tester', email='foo.bar@example.com')
```

#### gophish.models.Campaign

For each of the attributes in the campaign \(groups, template, page, etc.\), Gophish **only cares about the** `name`.

This means that you don't have to fetch the resources you want to use. You can simply create new ones with the correct name for the campaign \(see the example below\).

**Attributes**

* `id` \(int\) The result ID
* `results` \(list\(models.Result\)\) The campaign results
* `timeline` \(list\(models.TimelineEntry\)\) The timeline entries
* `name` \(str\) The campaign name
* `status` \(str\) The current status of the campaign
* `created_date` \(optional: datetime.datetime\) The campaign creation date
* `send_by_date` \(optional: datetime.datetime\) The date to send all emails by
* `launch_date` \(optional: datetime.datetime\) The scheduled time for campaign launch
* `template` \(models.Template\) The template to use in the campaign
* `page` \(models.Page\) The Landing Page to use in the campaign
* `smtp` \(models.SMTP\) The SMTP Profile to use when sending emails
* `url` \(str\) The URL to use when constructing links in phishing emails

**Methods**

* `__init__(self, **kwargs)` - Returns a new Campaign

  Example:

```python
groups = [Group(name='Existing Group')]
page = Page(name='Existing Page')
template = Template(name='Existing Template')
smtp = SMTP(name='Existing Profile')
url = 'http://phishing_server'

campaign = Campaign(
    name='Example Campaign', groups=groups, page=page,
    template=template, smtp=smtp)
```

#### gophish.models.Stat

Statistics for a campaign's results.

The Gophish API never requires you to create campaign stats. Instead, they are returned as part of the campaign summary objects.

**Attributes**

* `total` \(int\) The total number of targets in the campaign
* `sent` \(int\) The number of emails that were successfully sent
* `opened` \(int\) The number of emails that were opened
* `clicked` \(int\) The number of emails that were clicked by recipients in the campaign
* `submitted_data` \(int\) The number of captured credentials from the campaign
* `error` \(int\) The number of errors when sending emails in the campaign

#### gophish.models.CampaignSummary

A summarized view of a campaign. This is a lightweight high level view of campaign results, which can be quicker than retrieving full campaign details.

The Gophish API never requires you to create campaign summaries. Instead, they are returned when hitting the campaign summary endpoint.

**Attributes**

* `id` \(int\) The result ID
* `name` \(str\) The campaign name
* `created_date` \(optional: datetime.datetime\) The campaign creation date
* `send_by_date` \(optional: datetime.datetime\) The date to send all emails by
* `launch_date` \(optional: datetime.datetime\) The scheduled time for campaign launch
* `status` \(str\) The current status of the campaign
* `stats` \(list\(models.Stat\)\) The stats of campaign results

**Methods**

* `__init__(self, **kwargs)` - Returns a new CampaignSummary

  Example:

```python
summary = api.campaigns.summary(campaign_id=1)
print(summary.stats.as_dict())
```

### Methods

#### gophish.api.campaigns.get\(campaign\_id=None\)

Gets the details for one or more campaigns. To get a particular campaign, set the ID to the campaign ID.

If the `campaign_id` is not set, all campaigns owned by the current user will be returned.

**Returns**

* If the `campaign_id` is set: `models.Campaign`
* If `campaign_id` is `None`: `list(models.Campaign)`

#### gophish.api.campaigns.post\(campaign\)

Creates and launches a new campaign. This endpoint requires you to submit a `gophish.models.Campaign` object.

**Returns**

The `gophish.models.Campaign` object that was created.

#### gophish.api.campaigns.delete\(campaign\_id\)

Deletes the campaign specified by `campaign_id`.

**Returns**

A `gophish.models.Status` message.

#### gophish.api.campaigns.complete\(campaign\_id\)

Completes the campaign specified by `campaign_id`.

**Returns**

A `gophish.models.Status` message.

#### gophish.api.campaigns.summary\(campaign\_id=None\)

Gets the summaries for one or more campaigns. To get a particular campaign, set the ID to the campaign ID.

If the `campaign_id` is not set, the summary object for all campaigns owned by the current user will be returned.

**Returns**

* If the `campaign_id` is set: `models.CampaignSummary`
* If `campaign_id` is `None`: `models.CampaignSummaries`

### Examples

Here are some examples to show how to use the API.

All of these examples assume the following setup:

```python
from gophish import Gophish
from gophish.models import *

api_key = 'API_KEY'
api = Gophish(api_key)
```

#### Get All Campaigns

```python
campaigns = api.campaigns.get()
```

#### Get Single Campaign

```python
campaign = api.campaigns.get(campaign_id=1)
```

#### Create New Campaign

```python
groups = [Group(name='Existing Group')]
page = Page(name='Existing Page')
template = Template(name='Existing Template')
smtp = SMTP(name='Existing Profile')
url = 'http://phishing_server'
campaign = Campaign(
    name='Example Campaign', groups=groups, page=page,
    template=template, smtp=smtp)

campaign = api.campaigns.post(campaign)
print campaign.id
```

#### Get Campaign Summaries

```text
summaries = api.campaigns.summary()
summary = api.campaigns.summary(campaign_id=1)
```

