#Groups

The groups endpoint allows you to create, view, and manage Gophish groups.

## Table of Contents

* [Quick Example](#quick-example)
* [Models](#models)
* [Methods](#methods)

## Quick Example

This example shows how to retrieve the name and size of every group in Gophish.

``` python
from gophish import Gophish

api_key = 'API_KEY'
api = Gophish(api_key)

for group in api.groups.get():
print '{} has {} users'.format(group.name, len(group.targets))
```

### Models

#### gophish.models.User

##### Attributes

* `id` (int) The user ID
* `first_name` (str) The first name
* `last_name` (str) The last name
* `email` (str) The email address
* `position` (str) The position (job role)
##### Methods

* `__init__(self, **kwargs)` - Returns a new Target
Example:
``` python
result = Target(
first_name='Foo', last_name='Bar',
position='Tester', email='foo.bar@example.com')
```

#### gophish.models.Group

A group contains one or more `models.User` objects. The group name must be unique.

##### Attributes

* `id` (int) The group ID
* `targets` (list(models.User)) The group's users
* `name` (str) The group name
* `created_date` (optional: datetime.datetime) The group creation date
* `modified_date` (optional: datetime.datetime) The scheduled time for group launch

##### Methods

* `__init__(self, **kwargs)` - Returns a new Group

Example:

``` python
from gophish.models import *

targets = [
    Target(first_name='John', last_name='Doe', email='johndoe@example.com'),
    Target(first_name='Jane', last_name='Doe', email='janedoe@example.com')]
        
group = Group(name='Doe Company', targets=targets)
```

### Methods

#### gophish.api.groups.get(group_id=None)

Gets the details for one or more groups. To get a particular group, set the ID to the group ID.

If the `group_id` is not set, all groups owned by the current user will be returned.

**Returns**

* If the `group` is set: `models.Group`

* If `group_id` is `None`: `list(models.Group)`

#### gophish.api.groups.post(group)

Creates a new group. This endpoint requires you to submit a `gophish.models.Group` object.

**Returns**

The `gophish.models.Group` object that was created.

#### gophish.api.groups.put(group)

Edits an existing group. This endpoint requires you to submit a existing `gophish.models.Group` object with its `id` attribute set correctly.

**Returns**

The `gophish.models.Group` object that was edited.

#### gophish.api.groups.delete(group_id)

Deletes the group specified by `group_id`.

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

#### Get All Groups

``` python
groups = api.groups.get()
```

#### Get Single Group

``` python
group = api.groups.get(group_id=1)
```

#### Create New Group

``` python
targets = [
    Target(first_name='John', last_name='Doe', email='johndoe@example.com'),
    Target(first_name='Jane', last_name='Doe', email='janedoe@example.com')]
        
group = Group(name='Doe Company', targets=targets)
group = api.groups.post(group)
print group.id
```