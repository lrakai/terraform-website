---
layout: "extend"
page_title: "Home - Extending Terraform"
sidebar_current: "docs-extend-schemas"
description: |-
  Extending Terraform is a section for content dedicated to developing Plugins
  to extend Terraform's core offering.
---

# Terraform Schemas

Terraform Plugins are expressed using schemas to define attributes and their
behaviors, using a high level package exposed by Terraform Core named
[`schema`](https://github.com/hashicorp/terraform/tree/master/helper/schema).
Providers, Resources, and Provisioners all contains schemas, and Terraform Core
uses them to produce plan and apply executions based on the behaviors described. 

Below is an example `provider.go` file, detailing a hypothetical `ExampleProvider` implementation:

```go
package exampleprovider

import (
	"github.com/hashicorp/terraform/helper/schema"
	"github.com/hashicorp/terraform/terraform"
)

// Provider returns a terraform.ResourceProvider.
func Provider() terraform.ResourceProvider {
	// Example Provider requires an API Token.
	// The Email is optional
	return &schema.Provider{
		Schema: map[string]*schema.Schema{
			"api_token": {
				Type:        schema.TypeString,
				Required:    true,
			},
			"email": {
				Type:        schema.TypeString,
				Optional:    true,
				Default:     "",
			},
		},
	}
}
```

In this example we’re creating a `Provider` and setting it’s `schema`. This
schema is a collection of key value pairs of schema elements the attributes a
user can specify in their configuration. The keys are strings, and the values
are
[`schema.Schema`](https://github.com/hashicorp/terraform/blob/5727d3335247e5940af2bef35c88657753f6d260/helper/schema/schema.go#L37)
structs that define the behavior. 

Schemas can be thought of as a type paired one or more properties that describe
it’s behavior. 

## Schema Types

Schema items must be defined using one of the builtin types, such as
`TypeString`, `TypeBool`, `TypeInt`, et. al. The type defines what is considered
valid input for a given schema item in a users configuration. 

See [Schema
Types](/docs/extend/schema-types.html) for more information on the Types
available to schemas.

## Schema Properties

Schema items can have various properties that can be combined to match their
behavioras represented by their API. Some items are **Required**, others
**Optional**, while others may be **Computed** such that they are useful to be
tracked in state, but cannot be configured by users.

See [Schema Properties](/docs/extend/schema-properties.html) for more
information on the properties a schema can have.
