## Self Hosted TFC Agent on Managed Instance Group

This module handles the opinionated creation of infrastructure necessary to deploy Terraform Agent on MIG Container VMs.

This includes:

- Enabling necessary APIs
- VPC
- NAT & Cloud Router
- MIG Container Instance Template
- MIG Instance Manager
- FW Rules

Below are some examples:

### [Simple Self Hosted TFC Agent](../../examples/tfc-agent-mig-container-vm-simple/README.md)

This example shows how to deploy a Self Hosted TFC Agent on MIG Container VMs.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
README.md updated successfully
 <!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Requirements

Before this module can be used on a project, you must ensure that the following pre-requisites are fulfilled:

1. Required APIs are activated

    ```
    "iam.googleapis.com",
    "cloudresourcemanager.googleapis.com",
    "containerregistry.googleapis.com",
    "storage-component.googleapis.com",
    "logging.googleapis.com",
    "monitoring.googleapis.com"
    ```

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_google"></a> [google](#requirement\_google) | >= 3.53, < 5.0.0 |
| <a name="requirement_google-beta"></a> [google-beta](#requirement\_google-beta) | >= 3.53, < 5.0.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 3.4.3, < 4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google"></a> [google](#provider\_google) | >= 3.53, < 5.0.0 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 3.4.3, < 4.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_gce_container"></a> [gce\_container](#module\_gce\_container) | terraform-google-modules/container-vm/google | ~> 3.0 |
| <a name="module_mig"></a> [mig](#module\_mig) | terraform-google-modules/vm/google//modules/mig | ~> 7.0 |
| <a name="module_mig_template"></a> [mig\_template](#module\_mig\_template) | terraform-google-modules/vm/google//modules/instance_template | ~> 7.0 |

## Resources

| Name | Type |
|------|------|
| [google_compute_network.tfc_agent_network](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_network) | resource |
| [google_compute_router.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_router) | resource |
| [google_compute_router_nat.nat](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_router_nat) | resource |
| [google_compute_subnetwork.tfc_agent_subnetwork](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_subnetwork) | resource |
| [google_project_iam_binding.gce](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_binding) | resource |
| [google_service_account.tfc_agent_service_account](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account) | resource |
| [random_string.suffix](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project id to deploy Terraform Agent | `string` | n/a | yes |
| <a name="input_tfc_agent_token"></a> [tfc\_agent\_token](#input\_tfc\_agent\_token) | Terraform Cloud agent token. (mark as sensitive) (TFC Organization Settings >> Agents) | `string` | n/a | yes |
| <a name="input_additional_metadata"></a> [additional\_metadata](#input\_additional\_metadata) | Additional metadata to attach to the instance | `map(any)` | `{}` | no |
| <a name="input_cooldown_period"></a> [cooldown\_period](#input\_cooldown\_period) | The number of seconds that the autoscaler should wait before it starts collecting information from a new instance. | `number` | `60` | no |
| <a name="input_create_network"></a> [create\_network](#input\_create\_network) | When set to true, VPC,router and NAT will be auto created | `bool` | `true` | no |
| <a name="input_dind"></a> [dind](#input\_dind) | Flag to determine whether to expose dockersock | `bool` | `false` | no |
| <a name="input_image"></a> [image](#input\_image) | The Terraform Agent image | `string` | `"hashicorp/tfc-agent:latest"` | no |
| <a name="input_network_name"></a> [network\_name](#input\_network\_name) | Name for the VPC network | `string` | `"tfc-agent-network"` | no |
| <a name="input_region"></a> [region](#input\_region) | The GCP region to deploy instances into | `string` | `"us-east4"` | no |
| <a name="input_restart_policy"></a> [restart\_policy](#input\_restart\_policy) | The desired Docker restart policy for the agent image | `string` | `"Always"` | no |
| <a name="input_service_account"></a> [service\_account](#input\_service\_account) | Service account email address | `string` | `""` | no |
| <a name="input_startup_script"></a> [startup\_script](#input\_startup\_script) | User startup script to run when instances spin up | `string` | `""` | no |
| <a name="input_subnet_ip"></a> [subnet\_ip](#input\_subnet\_ip) | IP range for the subnet | `string` | `"10.10.10.0/24"` | no |
| <a name="input_subnet_name"></a> [subnet\_name](#input\_subnet\_name) | Name for the subnet | `string` | `"tfc-agent-subnet"` | no |
| <a name="input_subnetwork_project"></a> [subnetwork\_project](#input\_subnetwork\_project) | The ID of the project in which the subnetwork belongs. If it is not provided, the project\_id is used. | `string` | `""` | no |
| <a name="input_target_size"></a> [target\_size](#input\_target\_size) | The number of Terraform Cloud Agent instances | `number` | `2` | no |
| <a name="input_tfc_agent_address"></a> [tfc\_agent\_address](#input\_tfc\_agent\_address) | The HTTP or HTTPS address of the Terraform Cloud/Enterprise API. | `string` | `"https://app.terraform.io"` | no |
| <a name="input_tfc_agent_auto_update"></a> [tfc\_agent\_auto\_update](#input\_tfc\_agent\_auto\_update) | Controls automatic core updates behavior. Acceptable values include disabled, patch, and minor | `string` | `"minor"` | no |
| <a name="input_tfc_agent_name_prefix"></a> [tfc\_agent\_name\_prefix](#input\_tfc\_agent\_name\_prefix) | This name may be used in the Terraform Cloud user interface to help easily identify the agent | `string` | `"tfc-agent-container-vm"` | no |
| <a name="input_tfc_agent_single"></a> [tfc\_agent\_single](#input\_tfc\_agent\_single) | Enable single mode. This causes the agent to handle at most one job and<br>immediately exit thereafter. Useful for running agents as ephemeral<br>containers, VMs, or other isolated contexts with a higher-level scheduler<br>or process supervisor. | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_mig_instance_group"></a> [mig\_instance\_group](#output\_mig\_instance\_group) | The instance group url of the created MIG |
| <a name="output_mig_instance_template"></a> [mig\_instance\_template](#output\_mig\_instance\_template) | The name of the MIG Instance Template |
| <a name="output_mig_name"></a> [mig\_name](#output\_mig\_name) | The name of the MIG |
| <a name="output_network_name"></a> [network\_name](#output\_network\_name) | Name of VPC |
| <a name="output_service_account"></a> [service\_account](#output\_service\_account) | Service account email for GCE |
| <a name="output_subnet_name"></a> [subnet\_name](#output\_subnet\_name) | Name of VPC |
<!-- END_TF_DOCS -->