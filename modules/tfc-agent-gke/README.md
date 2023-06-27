## Self Hosted TFC Agent on GKE

This module handles the opinionated creation of infrastructure necessary to deploy Terraform Cloud Agent on GKE.

This includes:

- Enabling necessary APIs
- VPC
- GKE Cluster
- Kubernetes Secret

Below are some examples:

### [Simple Self Hosted TFC Agent on GKE](../../examples/tfc-agent-gke-simple/README.md)

This example shows how to deploy a simple GKE Self Hosted TFC Agent.

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
    "container.googleapis.com",
    "storage-component.googleapis.com",
    "logging.googleapis.com",
    "monitoring.googleapis.com"
    ```

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_google"></a> [google](#requirement\_google) | >= 4.3.0, < 5.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | ~> 2.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 3.4.3, < 4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google"></a> [google](#provider\_google) | >= 4.3.0, < 5.0 |
| <a name="provider_kubernetes"></a> [kubernetes](#provider\_kubernetes) | ~> 2.0 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 3.4.3, < 4.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_tfc_agent_cluster"></a> [tfc\_agent\_cluster](#module\_tfc\_agent\_cluster) | terraform-google-modules/kubernetes-engine/google//modules/beta-public-cluster/ | ~> 24.0 |

## Resources

| Name | Type |
|------|------|
| [google_compute_network.tfc_agent_network](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_network) | resource |
| [google_compute_subnetwork.tfc_agent_subnetwork](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_subnetwork) | resource |
| [google_project_iam_member.gke](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/project_iam_member) | resource |
| [kubernetes_secret.tfc_agent_secrets](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/secret) | resource |
| [random_string.suffix](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string) | resource |
| [google_client_config.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/client_config) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project id to deploy Terraform Cloud Agent cluster | `string` | n/a | yes |
| <a name="input_tfc_agent_token"></a> [tfc\_agent\_token](#input\_tfc\_agent\_token) | Terraform Cloud agent token. (mark as sensitive) (TFC Organization Settings >> Agents) | `string` | n/a | yes |
| <a name="input_create_network"></a> [create\_network](#input\_create\_network) | When set to true, VPC will be auto created | `bool` | `true` | no |
| <a name="input_ip_range_pods_cidr"></a> [ip\_range\_pods\_cidr](#input\_ip\_range\_pods\_cidr) | The secondary ip range cidr to use for pods | `string` | `"192.168.0.0/18"` | no |
| <a name="input_ip_range_pods_name"></a> [ip\_range\_pods\_name](#input\_ip\_range\_pods\_name) | The secondary ip range to use for pods | `string` | `"ip-range-pods"` | no |
| <a name="input_ip_range_services_cider"></a> [ip\_range\_services\_cider](#input\_ip\_range\_services\_cider) | The secondary ip range cidr to use for services | `string` | `"192.168.64.0/18"` | no |
| <a name="input_ip_range_services_name"></a> [ip\_range\_services\_name](#input\_ip\_range\_services\_name) | The secondary ip range to use for services | `string` | `"ip-range-scv"` | no |
| <a name="input_machine_type"></a> [machine\_type](#input\_machine\_type) | Machine type for TFC agent node pool | `string` | `"n1-standard-4"` | no |
| <a name="input_max_node_count"></a> [max\_node\_count](#input\_max\_node\_count) | Maximum number of nodes in the TFC agent node pool | `number` | `4` | no |
| <a name="input_min_node_count"></a> [min\_node\_count](#input\_min\_node\_count) | Minimum number of nodes in the TFC agent node pool | `number` | `2` | no |
| <a name="input_network_name"></a> [network\_name](#input\_network\_name) | Name for the VPC network | `string` | `"tfc-agent-network"` | no |
| <a name="input_region"></a> [region](#input\_region) | The GCP region to deploy instances into | `string` | `"us-west1"` | no |
| <a name="input_service_account"></a> [service\_account](#input\_service\_account) | Optional Service Account for the nodes | `string` | `""` | no |
| <a name="input_subnet_ip"></a> [subnet\_ip](#input\_subnet\_ip) | IP range for the subnet | `string` | `"10.0.0.0/17"` | no |
| <a name="input_subnet_name"></a> [subnet\_name](#input\_subnet\_name) | Name for the subnet | `string` | `"tfc-agent-subnet"` | no |
| <a name="input_subnetwork_project"></a> [subnetwork\_project](#input\_subnetwork\_project) | The ID of the project in which the subnetwork belongs. If it is not provided, the project\_id is used. | `string` | `""` | no |
| <a name="input_tfc_agent_address"></a> [tfc\_agent\_address](#input\_tfc\_agent\_address) | The HTTP or HTTPS address of the Terraform Cloud/Enterprise API. | `string` | `"https://app.terraform.io"` | no |
| <a name="input_tfc_agent_auto_update"></a> [tfc\_agent\_auto\_update](#input\_tfc\_agent\_auto\_update) | Controls automatic core updates behavior. Acceptable values include disabled, patch, and minor | `string` | `"minor"` | no |
| <a name="input_tfc_agent_k8s_secrets"></a> [tfc\_agent\_k8s\_secrets](#input\_tfc\_agent\_k8s\_secrets) | Name for the k8s secret required to configure TFC agent on GKE | `string` | `"tfc-agent-k8s-secrets"` | no |
| <a name="input_tfc_agent_name_prefix"></a> [tfc\_agent\_name\_prefix](#input\_tfc\_agent\_name\_prefix) | This name may be used in the Terraform Cloud user interface to help easily identify the agent | `string` | `"tfc-agent-k8s"` | no |
| <a name="input_tfc_agent_single"></a> [tfc\_agent\_single](#input\_tfc\_agent\_single) | Enable single mode. This causes the agent to handle at most one job and<br>immediately exit thereafter. Useful for running agents as ephemeral<br>containers, VMs, or other isolated contexts with a higher-level scheduler<br>or process supervisor. | `bool` | `false` | no |
| <a name="input_zones"></a> [zones](#input\_zones) | The GCP zone to deploy gke into | `list(string)` | <pre>[<br>  "us-west1-a"<br>]</pre> | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_ca_certificate"></a> [ca\_certificate](#output\_ca\_certificate) | The cluster ca certificate (base64 encoded) |
| <a name="output_client_token"></a> [client\_token](#output\_client\_token) | The bearer token for auth |
| <a name="output_cluster_name"></a> [cluster\_name](#output\_cluster\_name) | Cluster name |
| <a name="output_kubernetes_endpoint"></a> [kubernetes\_endpoint](#output\_kubernetes\_endpoint) | The cluster endpoint |
| <a name="output_location"></a> [location](#output\_location) | Cluster location |
| <a name="output_network_name"></a> [network\_name](#output\_network\_name) | Name of VPC |
| <a name="output_service_account"></a> [service\_account](#output\_service\_account) | The default service account used for TFC agent nodes. |
| <a name="output_subnet_name"></a> [subnet\_name](#output\_subnet\_name) | Name of VPC |
<!-- END_TF_DOCS -->