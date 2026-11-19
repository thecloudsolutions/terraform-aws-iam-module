<!-- markdownlint-disable -->
[![Release](https://img.shields.io/github/v/release/thecloudsolutions/terraform-aws-iam-module?style=for-the-badge&color=6150E8)](https://github.com/thecloudsolutions/terraform-aws-iam-module/releases)
[![Commit](https://img.shields.io/github/last-commit/thecloudsolutions/terraform-aws-iam-module?style=for-the-badge&color=FF9800)](https://github.com/thecloudsolutions/terraform-aws-iam-module/commits)
[![Issues](https://img.shields.io/github/issues/thecloudsolutions/terraform-aws-iam-module?style=for-the-badge&color=BB019E)](https://github.com/thecloudsolutions/terraform-aws-iam-module/issues)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-009FD2?style=for-the-badge)](#requirements)
[![License](https://img.shields.io/github/license/thecloudsolutions/terraform-aws-iam-module?style=for-the-badge&color=3FB950)](https://github.com/thecloudsolutions/terraform-aws-iam-module/blob/main/LICENSE)

# terraform-aws-iam-module

[![Cloud Solutions Banner][banner]](https://thecloudsolutions.com/)

[![Sponsor Us](https://img.shields.io/badge/sponsor_us-FF9800?style=for-the-badge)](https://github.com/sponsors/thecloudsolutions)
[![Hire Us](https://img.shields.io/badge/hire_us-009FD2?style=for-the-badge)](https://thecloudsolutions.com/contact-us)
[![Maintained By](https://img.shields.io/badge/maintained_by-Cloud_Solutions-6150E8?style=for-the-badge)](https://thecloudsolutions.com/)

## Description

Terraform module that provisions AWS IAM users, groups, roles, policies, instance profiles, and OpenID Connect providers in bulk. Supports configurable trust relationships, permission boundaries, and policy attachments.

## Variable Naming

In all Cloud Solutions **components** and **modules**, we intentionally use the _map suffix for input variables that accept map(object(...)).

For example:
```hcl
variable "iam_map" {
  type        = map(object({ ... }))
  description = "Keyed collection of iam definitions"
}
```
⚠️ This is a deliberate deviation from [Terraform Style Guide](https://developer.hashicorp.com/terraform/language/syntax/style).

We fully acknowledge that this naming convention does not follow the Terraform style guide, which recommends naming variables based on what they represent, not how they're implemented. We understand this, and want to emphasize:

1. This is a deliberate, informed decision, not an oversight.
2. Our primary goal is clarity and consistency for developers using our modules.
3. The intent is to give users immediate understanding of what kind of structure they're supplying.

### ✅ Why we made this decision (for our components and modules only)

This convention exists only within our components and modules structure. Here's why it works well for us:

1. Instant clarity for users - The _map suffix immediately signals that the variable supports multiple instances, each keyed by name. This avoids confusion around whether a variable is singular or plural.
2. Uniformity across all resources - Instead of struggling with awkward plurals (like kmses, rdses, vpcs, cloudfronts, account_managements) or inconsistent suffixes (_configs, _definitions, etc.), we use one simple, predictable pattern.
3. Avoids naming collisions - The _map suffix never conflicts with native Terraform or AWS resource types.
4. Future-proof for contributors - When contributors see _map, they can safely assume the structure is a map(object(...)) without checking variables.tf.

### 🙌 What this means for contributors

If you're contributing to Cloud Solutions components or modules, please:

1. Use the _map suffix when introducing a new map(object(...)) variable.
2. Follow the naming pattern consistently: rds_map, vpc_map, cloudfront_map, etc.
3. Don't apply this convention to Terraform projects outside components or modules - it's a local convention, not a global best practice.

If you have strong opinions or ideas for evolving this standard, you're welcome to open a discussion.

## Usage

```hcl
module "terraform-aws-iam-module" {
  source = "git::https://github.com/thecloudsolutions/terraform-aws-iam-module.git"
  # Cloud Solutions recommends to pin every module to a specific version
  # ?ref=x.x.x

  # Required variables
  iam_map = {
    key = <value>
  }
}
```

> [!IMPORTANT]
> In our examples we don't pin modules to specific versions to keep the documentation aligned with the latest releases.
> However, for your own projects, we recommend to pin each module to the exact version you're using. This approach helps maintain the stability of your infrastructure.
> Additionally, adopting a structured process for version updates will help prevent unexpected changes.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.15.8 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 6.60.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_iam"></a> [iam](#module\_iam) | git::https://github.com/thecloudsolutions/terraform-aws-iam-component.git | main |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_iam_map"></a> [iam\_map](#input\_iam\_map) | Keyed collection of iam definitions | <pre>map(object({<br/>    account_alias = optional(object({<br/>      account_alias = string<br/>    }))<br/><br/>    account_password_policy = optional(object({<br/>      allow_users_to_change_password = optional(bool)<br/>      hard_expiry                    = optional(bool)<br/>      max_password_age               = optional(number)<br/>      minimum_password_length        = optional(number)<br/>      password_reuse_prevention      = optional(number)<br/>      require_lowercase_characters   = optional(bool)<br/>      require_numbers                = optional(bool)<br/>      require_symbols                = optional(bool)<br/>      require_uppercase_characters   = optional(bool)<br/>    }))<br/><br/>    role = optional(object({<br/>      assume_role_policy                      = optional(string)<br/>      assume_role_policy_document_custom_name = optional(string)<br/>      description                             = optional(string)<br/>      force_detach_policies                   = optional(bool)<br/>      inline_policy = optional(set(object({<br/>        name   = string<br/>        policy = string<br/>      })), [])<br/><br/>      managed_policy_arns              = optional(set(string))<br/>      max_session_duration             = optional(number)<br/>      name                             = optional(string)<br/>      name_prefix                      = optional(string)<br/>      path                             = optional(string)<br/>      permissions_boundary             = optional(string)<br/>      permissions_boundary_custom_name = optional(string)<br/><br/>      tags = optional(map(string))<br/>    }))<br/><br/>    policy_documents = optional(map(object({<br/>      override_policy_documents = optional(list(string))<br/>      policy_id                 = optional(string)<br/>      source_policy_documents   = optional(list(string))<br/>      policy_version            = optional(string)<br/><br/>      statement = optional(list(object({<br/>        actions       = optional(set(string))<br/>        effect        = optional(string)<br/>        not_actions   = optional(set(string))<br/>        not_resources = optional(set(string))<br/>        resources     = optional(set(string))<br/>        sid           = optional(string)<br/><br/>        condition = optional(set(object({<br/>          test     = string<br/>          variable = string<br/>          values   = list(any)<br/>        })), [])<br/><br/>        principals = optional(set(object({<br/>          identifiers                = set(string)<br/>          saml_provider_custom_names = optional(set(string), [])<br/>          type                       = string<br/>        })), [])<br/><br/>        not_principals = optional(set(object({<br/>          identifiers                = set(string)<br/>          saml_provider_custom_names = optional(set(string), [])<br/>          type                       = string<br/>        })), [])<br/>      })), [])<br/>    })), {})<br/><br/>    role_policies = optional(map(object({<br/>      name                        = optional(string)<br/>      name_prefix                 = optional(string)<br/>      policy                      = optional(string)<br/>      policy_document_custom_name = optional(string)<br/>      role                        = optional(string)<br/>    })), {})<br/><br/>    role_policy_attachments = optional(list(object({<br/>      custom_name        = optional(string)<br/>      policy_arn         = optional(string)<br/>      policy_custom_name = optional(string)<br/>      role               = optional(string)<br/>    })), [])<br/><br/>    role_policies_exclusive = optional(object({<br/>      role_name           = optional(string)<br/>      policy_names        = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>    }))<br/><br/>    instance_profile = optional(object({<br/>      name        = optional(string)<br/>      name_prefix = optional(string)<br/>      path        = optional(string)<br/>      role        = optional(string)<br/>      tags        = optional(map(string))<br/>    }))<br/><br/>    policy_attachments_exclusive = optional(object({<br/>      policy_arns         = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>      role_name           = optional(string)<br/>    }))<br/><br/>    policies = optional(map(object({<br/>      policy                            = optional(string)<br/>      policy_custom_name                = optional(string)<br/>      delay_after_policy_creation_in_ms = optional(number)<br/>      description                       = optional(string)<br/>      name                              = optional(string)<br/>      name_prefix                       = optional(string)<br/>      path                              = optional(string)<br/>      tags                              = optional(map(string))<br/>    })), {})<br/><br/>    users = optional(map(object({<br/>      name                             = string<br/>      path                             = optional(string)<br/>      permissions_boundary             = optional(string)<br/>      permissions_boundary_custom_name = optional(string)<br/>      force_destroy                    = optional(bool)<br/>      tags                             = optional(map(string))<br/>    })), {})<br/><br/>    user_login_profiles = optional(map(object({<br/>      user                    = optional(string)<br/>      user_custom_name        = optional(string)<br/>      pgp_key                 = optional(string)<br/>      password_length         = optional(number)<br/>      password_reset_required = optional(bool)<br/>    })), {})<br/><br/>    access_keys = optional(map(object({<br/>      user             = optional(string)<br/>      user_custom_name = optional(string)<br/>      pgp_key          = optional(string)<br/>      status           = optional(string)<br/>    })), {})<br/><br/>    user_ssh_keys = optional(map(object({<br/>      username         = optional(string)<br/>      user_custom_name = optional(string)<br/>      encoding         = string<br/>      public_key       = string<br/>      status           = optional(string)<br/>    })), {})<br/><br/>    groups = optional(map(object({<br/>      name = string<br/>      path = optional(string)<br/>    })), {})<br/><br/>    user_policies = optional(map(object({<br/>      name                        = optional(string)<br/>      name_prefix                 = optional(string)<br/>      policy                      = optional(string)<br/>      policy_document_custom_name = optional(string)<br/>      user                        = optional(string)<br/>      user_custom_name            = optional(string)<br/>    })), {})<br/><br/>    user_policy_attachments = optional(map(object({<br/>      user               = optional(string)<br/>      user_custom_name   = optional(string)<br/>      policy_arn         = optional(string)<br/>      policy_custom_name = optional(string)<br/>    })), {})<br/><br/>    user_policies_exclusive = optional(map(object({<br/>      user_name           = optional(string)<br/>      user_custom_name    = optional(string)<br/>      policy_names        = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>    })), {})<br/><br/>    user_policy_attachments_exclusive = optional(map(object({<br/>      user_name           = optional(string)<br/>      user_custom_name    = optional(string)<br/>      policy_arns         = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>    })), {})<br/><br/>    group_policies = optional(map(object({<br/>      name                        = optional(string)<br/>      name_prefix                 = optional(string)<br/>      policy                      = optional(string)<br/>      policy_document_custom_name = optional(string)<br/>      group                       = optional(string)<br/>      group_custom_name           = optional(string)<br/>    })), {})<br/><br/>    group_policy_attachments = optional(map(object({<br/>      group              = optional(string)<br/>      group_custom_name  = optional(string)<br/>      policy_arn         = optional(string)<br/>      policy_custom_name = optional(string)<br/>    })), {})<br/><br/>    group_policies_exclusive = optional(map(object({<br/>      group_name          = optional(string)<br/>      group_custom_name   = optional(string)<br/>      policy_names        = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>    })), {})<br/><br/>    group_policy_attachments_exclusive = optional(map(object({<br/>      group_name          = optional(string)<br/>      group_custom_name   = optional(string)<br/>      policy_arns         = set(string)<br/>      policy_custom_names = optional(set(string), [])<br/>    })), {})<br/><br/>    group_memberships = optional(map(object({<br/>      name              = string<br/>      users             = set(string)<br/>      user_custom_names = optional(set(string), [])<br/>      group             = optional(string)<br/>      group_custom_name = optional(string)<br/>    })), {})<br/><br/>    user_group_memberships = optional(map(object({<br/>      user   = optional(string)<br/>      groups = optional(set(string), [])<br/>    })), {})<br/><br/>    openid_connect_providers = optional(map(object({<br/>      url             = string<br/>      client_id_list  = set(string)<br/>      thumbprint_list = optional(list(string))<br/>      tags            = optional(map(string))<br/>    })), {})<br/><br/>    saml_providers = optional(map(object({<br/>      name                   = string<br/>      saml_metadata_document = string<br/>      tags                   = optional(map(string))<br/>    })), {})<br/><br/>    service_linked_roles = optional(map(object({<br/>      aws_service_name = string<br/>      custom_suffix    = optional(string)<br/>      description      = optional(string)<br/>      tags             = optional(map(string))<br/>    })), {})<br/>  }))</pre> | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_iam_account_password_policy_expire_passwords"></a> [iam\_account\_password\_policy\_expire\_passwords](#output\_iam\_account\_password\_policy\_expire\_passwords) | Indicates whether passwords in the account expire. |
| <a name="output_iam_role_arns"></a> [iam\_role\_arns](#output\_iam\_role\_arns) | Amazon Resource Name (ARN) specifying the role. |
| <a name="output_iam_role_create_dates"></a> [iam\_role\_create\_dates](#output\_iam\_role\_create\_dates) | Creation date of the IAM role. |
| <a name="output_iam_role_ids"></a> [iam\_role\_ids](#output\_iam\_role\_ids) | ID of the role. |
| <a name="output_iam_role_names"></a> [iam\_role\_names](#output\_iam\_role\_names) | Name of the role. |
| <a name="output_iam_role_tags_all"></a> [iam\_role\_tags\_all](#output\_iam\_role\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_role_unique_ids"></a> [iam\_role\_unique\_ids](#output\_iam\_role\_unique\_ids) | Stable and unique string identifying the role. |
| <a name="output_iam_policy_document_ids"></a> [iam\_policy\_document\_ids](#output\_iam\_policy\_document\_ids) | The iam policy document id. |
| <a name="output_iam_policy_document_jsons"></a> [iam\_policy\_document\_jsons](#output\_iam\_policy\_document\_jsons) | Standard JSON policy document rendered based on the arguments above. |
| <a name="output_iam_role_policy_ids"></a> [iam\_role\_policy\_ids](#output\_iam\_role\_policy\_ids) | The role policy ID, in the form of role\_name:role\_policy\_name. |
| <a name="output_iam_role_policy_names"></a> [iam\_role\_policy\_names](#output\_iam\_role\_policy\_names) | The name of the policy. |
| <a name="output_iam_role_policy_policies"></a> [iam\_role\_policy\_policies](#output\_iam\_role\_policy\_policies) | The policy document attached to the role. |
| <a name="output_iam_role_policy_roles"></a> [iam\_role\_policy\_roles](#output\_iam\_role\_policy\_roles) | The name of the role associated with the policy. |
| <a name="output_iam_instance_profile_arns"></a> [iam\_instance\_profile\_arns](#output\_iam\_instance\_profile\_arns) | ARN assigned by AWS to the instance profile. |
| <a name="output_iam_instance_profile_create_dates"></a> [iam\_instance\_profile\_create\_dates](#output\_iam\_instance\_profile\_create\_dates) | Creation timestamp of the instance profile. |
| <a name="output_iam_instance_profile_ids"></a> [iam\_instance\_profile\_ids](#output\_iam\_instance\_profile\_ids) | Instance profile's ID. |
| <a name="output_iam_instance_profile_tags_all"></a> [iam\_instance\_profile\_tags\_all](#output\_iam\_instance\_profile\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_instance_profile_unique_ids"></a> [iam\_instance\_profile\_unique\_ids](#output\_iam\_instance\_profile\_unique\_ids) | Unique ID assigned by AWS. |
| <a name="output_iam_policy_arns"></a> [iam\_policy\_arns](#output\_iam\_policy\_arns) | ARN assigned by AWS to this policy. |
| <a name="output_iam_policy_names"></a> [iam\_policy\_names](#output\_iam\_policy\_names) | ARN assigned by AWS to this policy. |
| <a name="output_iam_policy_attachment_counts"></a> [iam\_policy\_attachment\_counts](#output\_iam\_policy\_attachment\_counts) | Number of entities (users, groups, and roles) that the policy is attached to. |
| <a name="output_iam_policy_ids"></a> [iam\_policy\_ids](#output\_iam\_policy\_ids) | ARN assigned by AWS to this policy. |
| <a name="output_iam_policy_policy_ids"></a> [iam\_policy\_policy\_ids](#output\_iam\_policy\_policy\_ids) | Policy's ID. |
| <a name="output_iam_policy_tags_all"></a> [iam\_policy\_tags\_all](#output\_iam\_policy\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_user_arns"></a> [iam\_user\_arns](#output\_iam\_user\_arns) | The ARN assigned by AWS for this user. |
| <a name="output_iam_user_ids"></a> [iam\_user\_ids](#output\_iam\_user\_ids) | The user's name. |
| <a name="output_iam_user_names"></a> [iam\_user\_names](#output\_iam\_user\_names) | The user's name. |
| <a name="output_iam_user_tags_all"></a> [iam\_user\_tags\_all](#output\_iam\_user\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_user_unique_ids"></a> [iam\_user\_unique\_ids](#output\_iam\_user\_unique\_ids) | The unique ID assigned by AWS. |
| <a name="output_iam_user_policy_ids"></a> [iam\_user\_policy\_ids](#output\_iam\_user\_policy\_ids) | The user policy ID, in the form of user\_name:user\_policy\_name. |
| <a name="output_iam_user_policy_names"></a> [iam\_user\_policy\_names](#output\_iam\_user\_policy\_names) | The name of the policy. |
| <a name="output_iam_user_policy_policies"></a> [iam\_user\_policy\_policies](#output\_iam\_user\_policy\_policies) | The policy document attached to the user. |
| <a name="output_iam_user_policy_users"></a> [iam\_user\_policy\_users](#output\_iam\_user\_policy\_users) | The name of the user associated with the policy. |
| <a name="output_iam_group_policy_ids"></a> [iam\_group\_policy\_ids](#output\_iam\_group\_policy\_ids) | The group policy ID, in the form of group\_name:group\_policy\_name. |
| <a name="output_iam_group_policy_names"></a> [iam\_group\_policy\_names](#output\_iam\_group\_policy\_names) | The name of the policy. |
| <a name="output_iam_group_policy_policies"></a> [iam\_group\_policy\_policies](#output\_iam\_group\_policy\_policies) | The policy document attached to the group. |
| <a name="output_iam_group_policy_groups"></a> [iam\_group\_policy\_groups](#output\_iam\_group\_policy\_groups) | The name of the group associated with the policy. |
| <a name="output_iam_user_login_profile_ids"></a> [iam\_user\_login\_profile\_ids](#output\_iam\_user\_login\_profile\_ids) | The IAM user's name. |
| <a name="output_iam_user_login_profile_passwords"></a> [iam\_user\_login\_profile\_passwords](#output\_iam\_user\_login\_profile\_passwords) | The generated password, available when pgp\_key is not provided. |
| <a name="output_iam_user_login_profile_key_fingerprints"></a> [iam\_user\_login\_profile\_key\_fingerprints](#output\_iam\_user\_login\_profile\_key\_fingerprints) | The fingerprint of the PGP key used to encrypt the password. |
| <a name="output_iam_user_login_profile_encrypted_passwords"></a> [iam\_user\_login\_profile\_encrypted\_passwords](#output\_iam\_user\_login\_profile\_encrypted\_passwords) | The encrypted password, base64 encoded. |
| <a name="output_iam_access_key_ids"></a> [iam\_access\_key\_ids](#output\_iam\_access\_key\_ids) | The access key ID. |
| <a name="output_iam_access_key_create_dates"></a> [iam\_access\_key\_create\_dates](#output\_iam\_access\_key\_create\_dates) | Date and time in RFC3339 format that the access key was created. |
| <a name="output_iam_access_key_encrypted_secrets"></a> [iam\_access\_key\_encrypted\_secrets](#output\_iam\_access\_key\_encrypted\_secrets) | The encrypted secret access key, base64 encoded. |
| <a name="output_iam_access_key_encrypted_ses_smtp_passwords_v4"></a> [iam\_access\_key\_encrypted\_ses\_smtp\_passwords\_v4](#output\_iam\_access\_key\_encrypted\_ses\_smtp\_passwords\_v4) | The encrypted SES SMTP password, base64 encoded. |
| <a name="output_iam_access_key_key_fingerprints"></a> [iam\_access\_key\_key\_fingerprints](#output\_iam\_access\_key\_key\_fingerprints) | The fingerprint of the PGP key used to encrypt the secret. |
| <a name="output_iam_access_key_secrets"></a> [iam\_access\_key\_secrets](#output\_iam\_access\_key\_secrets) | The secret access key, available when pgp\_key is not provided. |
| <a name="output_iam_access_key_ses_smtp_passwords_v4"></a> [iam\_access\_key\_ses\_smtp\_passwords\_v4](#output\_iam\_access\_key\_ses\_smtp\_passwords\_v4) | The secret access key converted into an SES SMTP password for the provider region. |
| <a name="output_iam_access_key_statuses"></a> [iam\_access\_key\_statuses](#output\_iam\_access\_key\_statuses) | The status of the access key. |
| <a name="output_iam_user_ssh_key_ids"></a> [iam\_user\_ssh\_key\_ids](#output\_iam\_user\_ssh\_key\_ids) | The unique identifier for the SSH public key. |
| <a name="output_iam_user_ssh_key_ssh_public_key_ids"></a> [iam\_user\_ssh\_key\_ssh\_public\_key\_ids](#output\_iam\_user\_ssh\_key\_ssh\_public\_key\_ids) | The unique identifier for the SSH public key. |
| <a name="output_iam_user_ssh_key_fingerprints"></a> [iam\_user\_ssh\_key\_fingerprints](#output\_iam\_user\_ssh\_key\_fingerprints) | The MD5 message digest of the SSH public key. |
| <a name="output_iam_user_ssh_key_statuses"></a> [iam\_user\_ssh\_key\_statuses](#output\_iam\_user\_ssh\_key\_statuses) | The status of the SSH public key. |
| <a name="output_iam_group_membership_names"></a> [iam\_group\_membership\_names](#output\_iam\_group\_membership\_names) | The name identifying the group membership. |
| <a name="output_iam_group_membership_users"></a> [iam\_group\_membership\_users](#output\_iam\_group\_membership\_users) | The IAM user names associated with the group. |
| <a name="output_iam_group_membership_groups"></a> [iam\_group\_membership\_groups](#output\_iam\_group\_membership\_groups) | The IAM group name associated with the membership. |
| <a name="output_iam_group_arns"></a> [iam\_group\_arns](#output\_iam\_group\_arns) | The ARN assigned by AWS for this group. |
| <a name="output_iam_group_ids"></a> [iam\_group\_ids](#output\_iam\_group\_ids) | The group's name. |
| <a name="output_iam_group_names"></a> [iam\_group\_names](#output\_iam\_group\_names) | The group's name. |
| <a name="output_iam_group_paths"></a> [iam\_group\_paths](#output\_iam\_group\_paths) | The path of the group in IAM. |
| <a name="output_iam_group_unique_ids"></a> [iam\_group\_unique\_ids](#output\_iam\_group\_unique\_ids) | The unique ID assigned by AWS. |
| <a name="output_iam_openid_connect_provider_arns"></a> [iam\_openid\_connect\_provider\_arns](#output\_iam\_openid\_connect\_provider\_arns) | The ARN assigned by AWS for this provider. |
| <a name="output_iam_openid_connect_provider_tags_all"></a> [iam\_openid\_connect\_provider\_tags\_all](#output\_iam\_openid\_connect\_provider\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_saml_provider_arns"></a> [iam\_saml\_provider\_arns](#output\_iam\_saml\_provider\_arns) | The ARN assigned by AWS for this provider. |
| <a name="output_iam_saml_provider_uuids"></a> [iam\_saml\_provider\_uuids](#output\_iam\_saml\_provider\_uuids) | Unique identifier assigned to the SAML provider. |
| <a name="output_iam_saml_provider_tags_all"></a> [iam\_saml\_provider\_tags\_all](#output\_iam\_saml\_provider\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_saml_provider_valid_untils"></a> [iam\_saml\_provider\_valid\_untils](#output\_iam\_saml\_provider\_valid\_untils) | The expiration date and time for the SAML provider in RFC3339 format. |
| <a name="output_iam_service_linked_role_arns"></a> [iam\_service\_linked\_role\_arns](#output\_iam\_service\_linked\_role\_arns) | Amazon Resource Name (ARN) specifying the role. |
| <a name="output_iam_service_linked_role_create_dates"></a> [iam\_service\_linked\_role\_create\_dates](#output\_iam\_service\_linked\_role\_create\_dates) | Creation date of the IAM role. |
| <a name="output_iam_service_linked_role_ids"></a> [iam\_service\_linked\_role\_ids](#output\_iam\_service\_linked\_role\_ids) | The Amazon Resource Name (ARN) of the role. |
| <a name="output_iam_service_linked_role_names"></a> [iam\_service\_linked\_role\_names](#output\_iam\_service\_linked\_role\_names) | Name of the role. |
| <a name="output_iam_service_linked_role_paths"></a> [iam\_service\_linked\_role\_paths](#output\_iam\_service\_linked\_role\_paths) | Path of the role. |
| <a name="output_iam_service_linked_role_tags_all"></a> [iam\_service\_linked\_role\_tags\_all](#output\_iam\_service\_linked\_role\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block. |
| <a name="output_iam_service_linked_role_unique_ids"></a> [iam\_service\_linked\_role\_unique\_ids](#output\_iam\_service\_linked\_role\_unique\_ids) | Stable and unique string identifying the role. |

## Quick Start

The project is under active development, and we welcome all contributors! 🎉
  
Before you start, please review our [Contributor Guidelines](https://github.com/thecloudsolutions/.github/blob/main/CONTRIBUTING.md) to understand the expectations and processes.
  
Follow these steps to get started:
  
---
  
### 1. **Fork the Repository**
- Click the **"Fork"** button on the repository's GitHub page to create a copy under your GitHub account.

> [!TIP]
> If you're new to GitHub, check out the [GitHub Docs](https://docs.github.com/en/get-started) to familiarize yourself with key concepts like forking, branching, and pull requests.
  
### 2. **Clone the Fork**
- Clone your fork to your local machine using:
  ```bash
  git clone https://github.com/<your-username>/<repository-name>.git
  ```
  
### 3. **Create a Branch**
- Create a new branch for your contribution:
  ```bash
  git checkout -b your-feature-name
  ```
  
### 4. **Make Changes**
- Implement your changes in the codebase.
- Ensure you follow the project's style guide and standards.
  
> [!WARNING]
> Always adhere to the project's coding standards and best practices outlined in the [CONTRIBUTING.md](https://github.com/thecloudsolutions/.github/blob/main/CONTRIBUTING.md) file.
    
### 5. **Test Your Changes**
- Run tests to verify your changes locally.
- Follow any project-specific testing instructions.
    
### 6. **Commit Your Changes**
- Stage and commit your changes with a clear and concise message:
  ```bash
  git add .
  git commit -m "Add description of the change"
  ```

> [!NOTE]
> Use meaningful commit messages in the present tense (e.g., "Add feature X" instead of "Added feature X").
  
### 7. **Push Your Branch**
- Push your branch to your fork:
  ```bash
  git push origin your-feature-name
  ```
    
### 8. **Open a Pull Request (PR)**
- Navigate to the original repository and click the **"New Pull Request"** button.
- Select your branch and submit your PR following the [Pull Request template](https://github.com/thecloudsolutions/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/pull_request_template.md).
- Use keywords like `Closes #123` to link your PR to a related issue.
  
### **🎉 Congratulations, you’ve made your first contribution! 🎉**  
We appreciate your effort and look forward to reviewing your work.

## About Us

**Cloud Solutions: Innovating Open Source for AWS & DevOps**

Cloud Solutions creates and maintains free, open-source tools to help startups thrive in the AWS Cloud and DevOps space. Our mission is to make advanced technology accessible to businesses of all sizes.

With years of experience and a commitment to innovation, we design solutions that simplify workflows, encourage collaboration, and drive growth - removing cost barriers along the way. Every contribution brings us closer to a future where technology empowers all.

Your support makes everything we do possible.

Thank you for being part of our journey to empower innovation and build a stronger Open Source future!

## Contributing

The project is under active development, and we welcome all contributors! 🎉 

Before you start, please review our [Contributor Guidelines](CONTRIBUTING.md) to understand the expectations and processes.

## Blog

Link to our [blog](https://thecloudsolutions.com/blog), where you can explore in-depth articles. Gain valuable insights and expert analysis on AWS cloud solution strategies and trends.

## License

The project is 100% Open Source and licensed under the [APACHE2](LICENSE).

## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## Copyrights

Copyright © 2020-2026 [Cloud Solutions](https://thecloudsolutions.com)

[banner]: https://github.com/thecloudsolutions/.github/blob/main/profile/banner.png