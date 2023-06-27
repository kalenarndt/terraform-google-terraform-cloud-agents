# Simple Self Hosted TFC Agent on GKE

## Overview

This example shows how to deploy TFC Agent on GKE.

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

1. Build the example TFC Agent image using Google Cloud Build. Alternatively, you can also use a prebuilt image or build using a local docker daemon.

    ```sh
    $ gcloud config set project $PROJECT_ID
    $ gcloud services enable cloudbuild.googleapis.com
    $ gcloud builds submit --config=cloudbuild.yaml
    ```

1. Replace image in [sample k8s deployment manifest](./sample-manifests/deployment.yaml).

    ```sh
    $ kustomize edit set image gcr.io/PROJECT_ID/tfc-agent:latest=gcr.io/$PROJECT_ID/tfc-agent:latest
    ```

1. Generate kubeconfig and apply the manifests for Deployment and HorizontalPodAutoscaler.

    ```sh
    $ gcloud container clusters get-credentials $(terraform output -raw cluster_name)
    $ kustomize build . | kubectl apply -f -
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
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | ~> 2.0 |
| <a name="requirement_tfe"></a> [tfe](#requirement\_tfe) | ~> 0.45.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google"></a> [google](#provider\_google) | ~> 4.0 |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | ~> 0.45.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_tfc_agent_gke"></a> [tfc\_agent\_gke](#module\_tfc\_agent\_gke) | ../../modules/tfc-agent-gke | n/a |

## Resources

| Name | Type |
|------|------|
| [tfe_agent_pool.tfc_agent_pool](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/agent_pool) | resource |
| [tfe_agent_token.tfc_agent_token](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/agent_token) | resource |
| [tfe_project.tfc_project](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/project) | resource |
| [tfe_workspace.tfc_workspace](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [google_client_config.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/client_config) | data source |
| [tfe_organization.tfc_org](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/organization) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project id to deploy Terraform Cloud Agent | `string` | n/a | yes |
| <a name="input_tfc_org_name"></a> [tfc\_org\_name](#input\_tfc\_org\_name) | Terraform Cloud org name where the agent pool will be created | `string` | n/a | yes |
| <a name="input_tfc_agent_pool_name"></a> [tfc\_agent\_pool\_name](#input\_tfc\_agent\_pool\_name) | Terraform Cloud Agent pool name to be created | `string` | `"tfc-agent-gke-simple-pool"` | no |
| <a name="input_tfc_agent_pool_token"></a> [tfc\_agent\_pool\_token](#input\_tfc\_agent\_pool\_token) | Terraform Cloud Agent pool token description | `string` | `"tfc-agent-gke-simple-pool-token"` | no |
| <a name="input_tfc_project_name"></a> [tfc\_project\_name](#input\_tfc\_project\_name) | Terraform Cloud project name to be created | `string` | `"GCP US West GKE"` | no |
| <a name="input_tfc_workspace_name"></a> [tfc\_workspace\_name](#input\_tfc\_workspace\_name) | Terraform Cloud workspace name to be created | `string` | `"tfc-agent-gke-simple"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_ca_certificate"></a> [ca\_certificate](#output\_ca\_certificate) | The cluster ca certificate (base64 encoded) |
| <a name="output_client_token"></a> [client\_token](#output\_client\_token) | The bearer token for auth |
| <a name="output_cluster_name"></a> [cluster\_name](#output\_cluster\_name) | Cluster name |
| <a name="output_kubernetes_endpoint"></a> [kubernetes\_endpoint](#output\_kubernetes\_endpoint) | The cluster endpoint |
| <a name="output_location"></a> [location](#output\_location) | Cluster location |
| <a name="output_network_name"></a> [network\_name](#output\_network\_name) | Name of VPC |
| <a name="output_service_account"></a> [service\_account](#output\_service\_account) | The default service account used for running nodes. |
| <a name="output_subnet_name"></a> [subnet\_name](#output\_subnet\_name) | Name of VPC |
<!-- END_TF_DOCS -->