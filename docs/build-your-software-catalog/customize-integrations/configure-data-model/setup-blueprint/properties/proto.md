---
sidebar_position: 13
description: Proto is a data type used to save proto definitions in Glint
sidebar_class_name: "custom-sidebar-item sidebar-property-string"
---

import ApiRef from "/docs/api-reference/\_learn_more_reference.mdx"

import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"

# Proto

Proto is a data type used to save .proto definitions in Glint

## 💡 Common proto usage

The proto property type can be used to store types defined in .proto files, for example:

- Messages between microservices;
- Microservices APIs;

## API definition

<Tabs groupId="api-definition" queryString defaultValue="basic" values={[
{label: "Basic", value: "basic"},
{label: "Array", value: "array"}
]}>

<TabItem value="basic">

```json showLineNumbers
{
  "myProtoProp": {
    "title": "My Proto",
    "icon": "My icon",
    "description": "My proto property",
    // highlight-start
    "type": "string",
    "format": "proto"
    // highlight-end
  }
}
```

</TabItem>
<TabItem value="array">

```json showLineNumbers
{
  "myProtoArray": {
    "title": "My proto array",
    "icon": "My icon",
    "description": "My proto array",
    // highlight-start
    "type": "array",
    "items": {
      "type": "string",
      "format": "proto"
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
      "myProtoProp" = {
        title      = "My proto"
        required   = false
        format     = "proto"
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
      "myProtoArray" = {
        identifier = "myProtoArray"
        title      = "My proto array"
        required   = false
        string_items = {
          format = "proto"
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

<Tabs groupId="pulumi-definition-proto-basic" queryString defaultValue="python" values={[
{label: "Python", value: "python"},
{label: "TypeScript", value: "typescript"},
{label: "JavaScript", value: "javascript"},
{label: "GO", value: "go"}
]}>

<TabItem value="python">

```python showLineNumbers
"""A Python Pulumi program"""

import pulumi
from port_pulumi import Blueprint,BlueprintPropertiesArgs,BlueprintPropertyArgs

blueprint = Blueprint(
    "myBlueprint",
    identifier="myBlueprint",
    title="My Blueprint",
    # highlight-start
    properties=BlueprintPropertiesArgs(
        string_props={
            "myProtoProp": BlueprintPropertyArgs(
                title="My proto",
                required=False,
                format="proto",
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
      myProtoProp: {
        title: "My proto",
        required: false,
        format: "proto",
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
      myProtoProp: {
        title: "My proto",
        required: false,
        format: "proto",
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
				"myProtoProp": glint.BlueprintPropertiesStringPropsArgs{
					Title:    pulumi.String("My proto"),
					Required: pulumi.Bool(false),
					Format:   pulumi.String("proto"),
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
