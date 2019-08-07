# terraform-google-folders

This module helps create several folders under the same parent, enforcing consistent permissions, and with a common naming convention.

The resources/services/activations/deletions that this module will create/trigger are:

- Create folders with the provided names
- Assign the defined permissions to the provided list of users or groups.

## Compatibility

 This module is meant for use with Terraform 0.12. If you haven't [upgraded](https://www.terraform.io/upgrade-guides/0-12.html)
  and need a Terraform 0.11.x-compatible version of this module, the last released version intended for
  Terraform 0.11.x is [0.1.0](https://registry.terraform.io/modules/terraform-google-modules/folders/google/0.1.0).


## Usage

Basic usage of this module is as follows:

```hcl
module "folders" {
  source  = "terraform-google-modules/folders/google"
  version = "~> 0.1"

  parent  = "folders/65552901371"

  names = [
    "dev",
    "staging",
    "production",
  ]

  set_roles = true

  per_folder_admins = [
    "group:gcp-developers@domain.com",
    "group:gcp-qa@domain.com",
    "group:gcp-ops@domain.com",
  ]

  all_folder_admins = [
    "group:gcp-security@domain.com",
  ]
}

```

Functional examples are included in the
[examples](./examples/) directory.

[^]: (autogen_docs_start)

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| all\_folder\_admins | List of IAM-style members that will get the extended permissions across all the folders. | list | `<list>` | no |
| folder\_admin\_roles | List of roles that will be applied to per folder owners on their respective folder. | list | `<list>` | no |
| names | Folder names. | list | `<list>` | no |
| parent | Path of the resource under which the folder will be placed (eg folders/12345). | string | `""` | no |
| per\_folder\_admins | List of IAM-style members per folder who will get extended permissions. | list | `<list>` | no |
| prefix | Optional prefix to enforce uniqueness of folder names. | string | `""` | no |
| set\_roles | Set roles to actors passed in role_members variable. | string | `"false"` | no |

## Outputs

| Name | Description |
|------|-------------|
| ids | Map of name => folder resource id. |
| names | Map of name => folder resource name. |

[^]: (autogen_docs_end)

## Requirements

These sections describe requirements for using this module.

### Software

The following dependencies must be available:

- [Terraform][terraform] v0.12
- [Terraform Provider for GCP][terraform-provider-gcp] plugin v2.0

### Service Account

A service account with the following roles must be used to provision
the resources of this module:

- Folder Creator: `roles/resourcemanager.folderCreator`

The [Project Factory module][project-factory-module] and the
[IAM module][iam-module] may be used in combination to provision a
service account with the necessary roles applied.

### APIs

A project with the following APIs enabled must be used to host the
resources of this module:

- Cloud Resource Manager API: `cloudresourcemanager.googleapis.com`

The [Project Factory module][project-factory-module] can be used to
provision a project with the necessary APIs enabled.

## Contributing

Refer to the [contribution guidelines](./CONTRIBUTING.md) for
information on contributing to this module.

[iam-module]: https://registry.terraform.io/modules/terraform-google-modules/iam/google
[project-factory-module]: https://registry.terraform.io/modules/terraform-google-modules/project-factory/google
[terraform-provider-gcp]: https://www.terraform.io/docs/providers/google/index.html
[terraform]: https://www.terraform.io/downloads.html