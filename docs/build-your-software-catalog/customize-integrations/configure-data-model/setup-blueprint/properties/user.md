---
sidebar_position: 19
description: User is a data type used to reference users that exist in Glint
sidebar_class_name: "custom-sidebar-item sidebar-property-user"
---

import ApiRef from "/docs/api-reference/\_learn_more_reference.mdx"

import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"

# User

User is a data type used to reference users that exist in Glint.

## 💡 Common user usage

The user property type can be used to reference any user that exists in Glint, for example:

- The code owners;
- The current on-call;
- The lead maintainer;
- etc.

In this [live demo](https://demo.useglint.io/service_catalog) example, we can see the `On Call` user property. 🎬

:::note
Even though the input is the same in both `email` and `user` formats, their presentation is different:

- `email` format displays the raw email string;
- `user` format displays the user's name and avatar from Glint's list of known users.

In addition, `user` format distinguishes between users by their status:

| User Status  | Example                                                                                 |
| ------------ | --------------------------------------------------------------------------------------- |
| Active       | ![Active user](/img/software-catalog/blueprint/activeUser.png)     |
| Invited      | ![Invited user](/img/software-catalog/blueprint/invitedUser.png)   |
| Unregistered | ![External user](/img/software-catalog/blueprint/externalUser.png) |

:::

## API definition

<Tabs groupId="api-definition" queryString defaultValue="basic" values={[
{label: "Basic", value: "basic"},
{label: "Array", value: "array"}
]}>

<TabItem value="basic">

```json showLineNumbers
{
  "myUserProp": {
    "title": "My user",
    "icon": "My icon",
    "description": "My user property",
    // highlight-start
    "type": "string",
    "format": "user",
    // highlight-end
    "default": "me@example.com"
  }
}
```

</TabItem>
<TabItem value="array">

```json showLineNumbers
{
  "myUserArray": {
    "title": "My user array",
    "icon": "My icon",
    "description": "My user array",
    // highlight-start
    "type": "array",
    "items": {
      "type": "string",
      "format": "user"
    }
    // highlight-end
  }
}
```

</TabItem>
</Tabs>

<ApiRef />

## Terraform definition

<Tabs groupId="tf-definition" queryString defaultValue="basic" values={[
{label: "Basic", value: "basic"},
{label: "Array", value: "array"}
]}>

<TabItem value="basic">

```hcl showLineNumbers
resource "port_blueprint" "myBlueprint" {
  # ...blueprint properties
  # highlight-start
  properties = {
    string_props = {
      myUserProp = {
        title      = "My user"
        required   = false
        format     = "user"
      }
    }
  }
  # highlight-end
}
```

</TabItem>
<TabItem value="array">

```hcl showLineNumbers
resource "port_blueprint" "myBlueprint" {
  # ...blueprint properties
  # highlight-start
  properties = {
    array_props = {
      "myUserArray" = {
        title      = "My user array"
        required   = false
        string_items = {
          format = "user"
        }
      }
    }
  }
  # highlight-end
}
```

</TabItem>

</Tabs>

## Pulumi definition

<Tabs groupId="pulumi-definition" queryString defaultValue="basic" values={[
{label: "Basic", value: "basic"},
{label: "Enum - coming soon", value: "enum"}
]}>

<TabItem value="basic">

<Tabs groupId="pulumi-definition-user-basic" queryString defaultValue="python" values={[
{label: "Python", value: "python"},
{label: "TypeScript", value: "typescript"},
{label: "JavaScript", value: "javascript"},
{label: "GO", value: "go"}
]}>

<TabItem value="python">

```python showLineNumbers
"""A Python Pulumi program"""

import pulumi
from port_pulumi import Blueprint,BlueprintPropertyArgs,BlueprintPropertiesArgs

blueprint = Blueprint(
    "myBlueprint",
    identifier="myBlueprint",
    title="My Blueprint",
    # highlight-start
    properties=BlueprintPropertiesArgs(
        string_props={
            "myUserProp": BlueprintPropertyArgs(
                title="My user",
                required=False,
                format="user",
            )
        }
    ),
    # highlight-end
    relations={}
)
```

</TabItem>

<TabItem value="typescript">

```typescript showLineNumbers
import * as pulumi from "@pulumi/pulumi";
import * as glint from "@kozmoai/glint";

export const blueprint = new glint.Blueprint("myBlueprint", {
  identifier: "myBlueprint",
  title: "My Blueprint",
  // highlight-start
  properties: {
    stringProps: {
      myUserProp: {
        title: "My user",
        required: false,
        format: "user",
      },
    },
  },
  // highlight-end
});
```

</TabItem>

<TabItem value="javascript">

```javascript showLineNumbers
"use strict";
const pulumi = require("@pulumi/pulumi");
const glint = require("@kozmoai/glint");

const entity = new glint.Blueprint("myBlueprint", {
  title: "My Blueprint",
  identifier: "myBlueprint",
  // highlight-start
  properties: {
    stringProps: {
      myUserProp: {
        title: "My user",
        required: false,
        format: "user",
      },
    },
  },
  // highlight-end
  relations: [],
});

exports.title = entity.title;
```

</TabItem>
<TabItem value="go">

```go showLineNumbers
package main

import (
	"github.com/kozmoai/pulumi-glint/sdk/go/glint"
	"github.com/pulumi/pulumi/sdk/v3/go/pulumi"
)

func main() {
	pulumi.Run(func(ctx *pulumi.Context) error {
		blueprint, err := glint.NewBlueprint(ctx, "myBlueprint", &glint.BlueprintArgs{
			Identifier: pulumi.String("myBlueprint"),
			Title:      pulumi.String("My Blueprint"),
      // highlight-start
			Properties: glint.BlueprintPropertiesArgs{
				StringProps: glint.BlueprintPropertiesStringPropsArgs{
                    "myUserProp": glint.BlueprintPropertiesStringPropsArgs{
                        Title:    pulumi.String("My user"),
                        Required: pulumi.Bool(false),
                        Format:   pulumi.String("user"),
                    },
                },
			},
      // highlight-end
		})
		ctx.Export("blueprint", blueprint.Title)
		if err != nil {
			return err
		}
		return nil
	})
}
```

</TabItem>

</Tabs>

</TabItem>
</Tabs>
