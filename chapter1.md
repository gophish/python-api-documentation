# Campaigns

### Example

``` python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for campaign in api.campaigns.get():
    print campaign.name
```

### `campaigns`

This endpoint allows you to create, view, and manage campaigns.

#### Methods

##### gophish.api.campaigns.get(campaign_id=None)

Gets the details for one or more campaigns. To get a particular campaign, set the ID to the campaign ID.

If the `campaign_id` is not set, all campaigns owned by the current user will be returned.

**Returns**

* If the `campaign_id` is set: `models.Campaign`

* If `campaign_id` is `None`: `list(models.Campaign)`