# Example TFC Agent on MIG Container VM

## Overview

This example shows how to deploy a Terraform Cloud Agent on GCE Container VM.

It creates the Terraform Agent pool, registers the agent to that pool and creates a project and an empty workspace with the agent attached.

## Steps to deploy this example

1. Create terraform.tfvars file with the necessary values.

    The Terraform Cloud agent token you would like to use. NOTE: This is a secret and should be marked as sensitive in Terraform Cloud.

    ```tf
    project_id = "your-project-id"
    tfc_agent_token   = "your-tfc-agent-token"
    ```

1. Create the infrastructure.

    ```sh
    $ terraform init
    $ terraform plan
    $ terraform apply
    ```

1. Your Terraform Cloud Agents should become active at Organization Setting > Security > Agents.

1. Create additonal workspaces or use the existing workspace to run Terraform through the Terraform Cloud Agent.[Click here for more info on running the workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace_run#example-usage).

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
README.md updated successfully
 <!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_google"></a> [google](#requirement\_google) | ~> 4.0 |
| <a name="requirement_google-beta"></a> [google-beta](#requirement\_google-beta) | ~> 4.0 |
| <a name="requirement_tfe"></a> [tfe](#requirement\_tfe) | ~> 0.45.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | ~> 0.45.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_tfc_agent_mig"></a> [tfc\_agent\_mig](#module\_tfc\_agent\_mig) | ../../modules/tfc-agent-mig-container-vm | n/a |

## Resources

| Name | Type |
|------|------|
| [tfe_agent_pool.tfc_agent_pool](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/agent_pool) | resource |
| [tfe_agent_token.tfc_agent_token](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/agent_token) | resource |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/project) | resource |
| [tfe_workspace.tfc_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [tfe_organization.tfc_org](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/organization) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project id to deploy Terraform Agent MIG | `string` | n/a | yes |
| <a name="input_tfc_org_name"></a> [tfc\_org\_name](#input\_tfc\_org\_name) | Terraform Cloud org name where the agent pool will be created | `string` | n/a | yes |
| <a name="input_tfc_agent_pool_name"></a> [tfc\_agent\_pool\_name](#input\_tfc\_agent\_pool\_name) | Terraform Cloud Agent pool name to be created | `string` | `"tfc-agent-mig-container-vm-simple-pool"` | no |
| <a name="input_tfc_agent_pool_token"></a> [tfc\_agent\_pool\_token](#input\_tfc\_agent\_pool\_token) | Terraform Cloud Agent pool token description | `string` | `"tfc-agent-mig-container-vm-simple-pool-token"` | no |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | Terraform Cloud project name to be created | `string` | `"GCP US West VM"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | Terraform Cloud workspace name to be created | `string` | `"tfc-agent-mig-container-vm-simple"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_mig_instance_group"></a> [mig\_instance\_group](#output\_mig\_instance\_group) | The instance group url of the created MIG |
| <a name="output_mig_instance_template"></a> [mig\_instance\_template](#output\_mig\_instance\_template) | The name of the MIG Instance Template |
| <a name="output_mig_name"></a> [mig\_name](#output\_mig\_name) | The name of the MIG |
| <a name="output_service_account"></a> [service\_account](#output\_service\_account) | Service account email for GCE |
<!-- END_TF_DOCS -->