# F5-XC-HEALTHCHECK
This repository consists of Terraform templates to bring up a F5XC AWC VPC multi node environment.

## Usage

- Clone this repo with: `git clone --recurse-submodules https://github.com/cklewar/f5-xc-healthcheck`
- Enter repository directory with: `cd f5-xc-healthcheck`
- Obtain F5XC API certificate file from Console and save it to `cert` directory
- Pick and choose from below examples and add mandatory input data and copy data into file `main.tf.example`.
- Rename file __main.tf.example__ to __main.tf__ with: `rename main.tf.example main.tf`
- Initialize with: `terraform init`
- Apply with: `terraform apply -auto-approve` or destroy with: `terraform destroy -auto-approve`

## Multi Node Single NIC and new subnet module usage example

````hcl
variable "project_prefix" {
  type        = string
  description = "prefix string put in front of string"
  default     = "f5xc"
}

variable "project_suffix" {
  type        = string
  description = "prefix string put at the end of string"
  default     = "01"
}

variable "f5xc_api_p12_file" {
  type = string
}

variable "f5xc_api_url" {
  type = string
}

variable "f5xc_tenant" {
  type = string
}

module "healthcheck_http" {
  source                = "./modules/f5xc/healthcheck"
  f5xc_tenant           = var.f5xc_tenant
  f5xc_namespace        = "system"
  f5xc_api_url          = var.f5xc_api_url
  f5xc_api_p12_file     = var.f5xc_api_p12_file
  f5xc_healthcheck_type = "http"
  f5xc_healthcheck_name = format("%s-hc-%s", var.project_prefix, var.project_suffix)
  f5xc_healthcheck_http_headers = { "key1" = "h1", "value" = "v1", "key2" = "h2", "value" = "v2", "key3" = "h3", "value" = "v3" }
}

module "healthcheck_tcp" {
  source                        = "./modules/f5xc/healthcheck"
  f5xc_tenant                   = var.f5xc_tenant
  f5xc_namespace                = "system"
  f5xc_api_url                  = var.f5xc_api_url
  f5xc_api_p12_file             = var.f5xc_api_p12_file
  f5xc_healthcheck_type         = "tcp"
  f5xc_healthcheck_name         = format("%s-hc-%s", var.project_prefix, var.project_suffix)
  f5xc_healthcheck_send_payload = "000000FF"
}
```
