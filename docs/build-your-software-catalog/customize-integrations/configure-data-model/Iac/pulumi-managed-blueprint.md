---
sidebar_position: 1
title: Pulumi
description: Comprehensive blueprint with properties, relations and mirror properties
---

import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"

# Pulumi-Managed Blueprint Example

This example includes a complete [blueprint](/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/setup-blueprint.md) resource definition in Pulumi, which includes:

- [Blueprint](/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/setup-blueprint.md?definition=pulumi#configure-blueprints-in-glint) definition examples;
- All [property](/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/properties/properties.md) type definitions;
- [Relation](/build-your-software-catalog/customize-integrations/configure-data-model/relate-blueprints/relate-blueprints.md?definition=pulumi#configure-relations-in-glint) definition example;
- [Mirror property](/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/properties/mirror-property) definition example;
- [Calculation property](/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/properties/calculation-property/calculation-property.md) definition example.

Here is the example definition in all the supported languages:

<Tabs groupId="pulumi-definition" queryString defaultValue="python" values={[
{label: "Python", value: "python"},
{label: "TypeScript", value: "typescript"},
{label: "JavaScript", value: "javascript"},
{label: "GO", value: "go"}
]}>

<TabItem value="python">

```python showLineNumbers
from pulumi import ResourceOptions
import port_pulumi as glint

other = glint.Blueprint(
    "other",
    identifier="test-docs-relation",
    icon="Microservice",
    title="Test Docs Relation",
    properties=glint.BlueprintPropertiesArgs(
        string_props={
            "myStringProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My string", required=False
            )
        }
    ),
)

blueprint = glint.Blueprint(
    "myBlueprint",
    identifier="test-docs",
    icon="Microservice",
    title="Test Docs",
    properties=glint.BlueprintPropertiesArgs(
        string_props={
            "myStringProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My string", required=False
            ),
            "myUrlProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My url", required=False, format="url"
            ),
            "myEmailProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My email", required=False, format="email"
            ),
            "myUserProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My user", required=False, format="user"
            ),
            "myTeamProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My team", required=False, format="team"
            ),
            "myDatetimeProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My datetime", required=False, format="date-time"
            ),
            "myTimerProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My timer", required=False, format="timer"
            ),
            "myYAMLProp": glint.BlueprintPropertiesStringPropsArgs(
                title="My yaml", required=False, format="yaml"
            ),
        },
        number_props={
            "myNumberProp": glint.BlueprintPropertiesNumberPropsArgs(
                title="My number", required=False,
            )
        },
        boolean_props={
            "myBooleanProp": glint.BlueprintPropertiesBooleanPropsArgs(
                title="My boolean", required=False
            )
        },
        object_props={
            "myObjectProp": glint.BlueprintPropertiesObjectPropsArgs(
                title="My object", required=False
            )
        },
        array_props={
            "myArrayProp": glint.BlueprintPropertiesArrayPropsArgs(
                title="My array", required=False
            )
        }
    ),
    mirror_properties={
        "myMirrorProp": glint.BlueprintMirrorPropertiesArgs(
            title="My mirror property", path="myRelation.myStringProp"
        ),
        "myMirrorPropWithMeta": glint.BlueprintMirrorPropertiesArgs(
            title="My mirror property of meta property", path="myRelation.$identifier"
        ),
    },
    calculation_properties={
        "myCalculation": glint.BlueprintCalculationPropertiesArgs(
            title="My calculation property", calculation=".properties.myStringProp + .properties.myStringProp", type="string",
        ),
        "myCalculationWithMeta": glint.BlueprintCalculationPropertiesArgs(
            title="My calculation property with meta properties", calculation='.identifier + "-" + .title + "-" + .properties.myStringProp', type="string",
        ),
    },
    relations={
        "myRelation": glint.BlueprintRelationsArgs(
            title="My relation", target="test-docs-relation", many=False, required=False,
        ),
    },
    opts=ResourceOptions(depends_on=[other]),
)
```

</TabItem>

<TabItem value="typescript">

```typescript showLineNumbers
import * as pulumi from "@pulumi/pulumi";
import * as glint from "@kozmoai/glint";

const other = new glint.Blueprint("other", {
  identifier: "test-docs-relation",
  icon: "Microservice",
  title: "Test Docs Relation",
  properties: {
    stringProps: {
      myStringProp: {
        title: "My string",
        required: false,
      },
    },
  },
});

const myBlueprint = new glint.Blueprint(
  "myBlueprint",
  {
    identifier: "test-docs",
    icon: "Microservice",
    title: "Test Docs",
    properties: {
      stringProps: {
        myStringProp: {
          title: "My string",
          required: false,
        },
        myUrlProp: {
          title: "My url",
          required: false,
          format: "url",
        },
        myEmailProp: {
          title: "My email",
          required: false,
          format: "email",
        },
        myUserProp: {
          title: "My user",
          required: false,
          format: "user",
        },
        myTeamProp: {
          title: "My team",
          required: false,
          format: "team",
        },
        myDatetimeProp: {
          title: "My datetime",
          required: false,
          format: "date-time",
        },
        myTimerProp: {
          title: "My timer",
          required: false,
          format: "timer",
        },
        myYAMLProp: {
          title: "My yaml",
          required: false,
          format: "yaml",
        },
      },
      numberProps: {
        myNumberProp: {
          title: "My number",
          required: false,
        },
      },
      booleanProps: {
        myBooleanProp: {
          title: "My boolean",
          required: false,
        },
      },
      objectProps: {
        myObjectProp: {
          title: "My object",
          required: false,
        },
      },
      arrayProps: {
        myArrayProp: {
          title: "My array",
          required: false,
        },
      },
    },
    mirrorProperties: {
      myMirrorProp: {
        title: "My mirror property",
        path: "myRelation.myStringProp",
      },
      myMirrorPropWithMeta: {
        title: "My mirror property of meta property",
        path: "myRelation.$identifier",
      },
    },
    calculationProperties: {
      myCalculation: {
        title: "My calculation property",
        calculation: ".properties.myStringProp + .properties.myStringProp",
        type: "string",
      },
      myCalculationWithMeta: {
        title: "My calculation property with meta properties",
        calculation:
          '.identifier + "-" + .title + "-" + .properties.myStringProp',
        type: "string",
      },
    },
    relations: {
      myRelation: {
        title: "My relation",
        target: "test-docs-relation",
        many: false,
        required: false,
      },
    },
  },
  { dependsOn: [other] }
);
```

</TabItem>

<TabItem value="javascript">

```javascript showLineNumbers
"use strict";
const pulumi = require("@pulumi/pulumi");
const glint = require("@kozmoai/glint");

const other = new glint.Blueprint("other", {
  identifier: "test-docs-relation",
  icon: "Microservice",
  title: "Test Docs Relation",
  properties: {
    stringProps: {
      myStringProp: {
        title: "My string",
        required: false,
      },
    },
  },
});

const myBlueprint = new glint.Blueprint(
  "myBlueprint",
  {
    identifier: "test-docs",
    icon: "Microservice",
    title: "Test Docs",
    properties: {
      stringProps: {
        myStringProp: {
          title: "My string",
          required: false,
        },
        myUrlProp: {
          title: "My url",
          required: false,
          format: "url",
        },
        myEmailProp: {
          title: "My email",
          required: false,
          format: "email",
        },
        myUserProp: {
          title: "My user",
          required: false,
          format: "user",
        },
        myTeamProp: {
          title: "My team",
          required: false,
          format: "team",
        },
        myDatetimeProp: {
          title: "My datetime",
          required: false,
          format: "date-time",
        },
        myTimerProp: {
          title: "My timer",
          required: false,
          format: "timer",
        },
        myYAMLProp: {
          title: "My yaml",
          required: false,
          format: "yaml",
        },
      },
      numberProps: {
        myNumberProp: {
          title: "My number",
          required: false,
        },
      },
      booleanProps: {
        myBooleanProp: {
          title: "My boolean",
          required: false,
        },
      },
      objectProps: {
        myObjectProp: {
          title: "My object",
          required: false,
        },
      },
      arrayProps: {
        myArrayProp: {
          title: "My array",
          required: false,
        },
      },
    },
    mirrorProperties: {
      myMirrorProp: {
        title: "My mirror property",
        path: "myRelation.myStringProp",
      },
      myMirrorPropWithMeta: {
        title: "My mirror property of meta property",
        path: "myRelation.$identifier",
      },
    },
    calculationProperties: {
      myCalculation: {
        title: "My calculation property",
        calculation: ".properties.myStringProp + .properties.myStringProp",
        type: "string",
      },
      myCalculationWithMeta: {
        title: "My calculation property with meta properties",
        calculation:
          '.identifier + "-" + .title + "-" + .properties.myStringProp',
        type: "string",
      },
    },
    relations: {
      myRelation: {
        title: "My relation",
        target: "test-docs-relation",
        many: false,
        required: false,
      },
    },
  },
  { dependsOn: [other] }
);
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

		other, err := glint.NewBlueprint(ctx, "other", &glint.BlueprintArgs{
			Identifier: pulumi.String("test-docs-relation"),
			Icon:       pulumi.String("Microservice"),
			Title:      pulumi.String("Test Docs Relation"),
			Properties: glint.BlueprintPropertiesArgs{
				StringProps: glint.BlueprintPropertiesStringPropsMap{
					"myStringProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My string"),
						Required: pulumi.Bool(false),
					},
				},
			},
		})
		if err != nil {
			return err
		}

		myBlueprint, err := glint.NewBlueprint(ctx, "myBlueprint", &glint.BlueprintArgs{
			Identifier: pulumi.String("test-docs"),
			Icon:       pulumi.String("Microservice"),
			Title:      pulumi.String("Test Docs"),
			Properties: glint.BlueprintPropertiesArgs{
				StringProps: glint.BlueprintPropertiesStringPropsMap{
					"myStringProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My string"),
						Required: pulumi.Bool(false),
					},
					"myUrlProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My url"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("url"),
					},
					"myEmailProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My email"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("email"),
					},
					"myUserProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My user"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("user"),
					},
					"myTeamProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My team"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("team"),
					},
					"myDatetimeProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My datetime"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("date-time"),
					},
					"myTimerProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My timer"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("timer"),
					},
					"myYAMLProp": glint.BlueprintPropertiesStringPropsArgs{
						Title:    pulumi.String("My yaml"),
						Required: pulumi.Bool(false),
						Format:   pulumi.String("yaml"),
					},
				},
				NumberProps: glint.BlueprintPropertiesNumberPropsMap{
					"myNumberProp": glint.BlueprintPropertiesNumberPropsArgs{
						Title:    pulumi.String("My number"),
						Required: pulumi.Bool(false),
					},
				},
				BooleanProps: glint.BlueprintPropertiesBooleanPropsMap{
					"myBooleanProp": glint.BlueprintPropertiesBooleanPropsArgs{
						Title:    pulumi.String("My boolean"),
						Required: pulumi.Bool(false),
					},
				},
				ObjectProps: glint.BlueprintPropertiesObjectPropsMap{
					"myObjectProp": glint.BlueprintPropertiesObjectPropsArgs{
						Title:    pulumi.String("My object"),
						Required: pulumi.Bool(false),
					},
				},
				ArrayProps: glint.BlueprintPropertiesArrayPropsMap{
					"myArrayProp": glint.BlueprintPropertiesArrayPropsArgs{
						Title:    pulumi.String("My array"),
						Required: pulumi.Bool(false),
					},
				},
			},
			MirrorProperties: glint.BlueprintMirrorPropertiesMap{
				"myMirrorProp": glint.BlueprintMirrorPropertiesArgs{
					Title: pulumi.String("My mirror property"),
					Path:  pulumi.String("myRelation.myStringProp"),
				},
				"myMirrorPropWithMeta": glint.BlueprintMirrorPropertiesArgs{
					Title: pulumi.String("My mirror property of meta property"),
					Path:  pulumi.String("myRelation.$identifier"),
				},
			},
			CalculationProperties: glint.BlueprintCalculationPropertiesMap{
				"myCalculation": glint.BlueprintCalculationPropertiesArgs{
					Title:       pulumi.String("My calculation property"),
					Calculation: pulumi.String(".properties.myStringProp + .properties.myStringProp"),
					Type:        pulumi.String("string"),
				},
				"myCalculationWithMeta": glint.BlueprintCalculationPropertiesArgs{
					Title:       pulumi.String("My calculation property with meta properties"),
					Calculation: pulumi.String(".identifier + \"-\" + .title + \"-\" + .properties.myStringProp"),
					Type:        pulumi.String("string"),
				},
			},
			Relations: glint.BlueprintRelationsMap{
				"myRelation": &glint.BlueprintRelationsArgs{
					Title:    pulumi.String("My relation"),
					Target:   pulumi.String("test-docs-relation"),
					Many:     pulumi.Bool(false),
					Required: pulumi.Bool(false),
				},
			},
		}, pulumi.DependsOn([]pulumi.Resource{other}))

		if err != nil {
			return err
		}
		return nil
	})
}
```

</TabItem>

</Tabs>
