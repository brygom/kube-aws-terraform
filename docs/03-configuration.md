Here are configurations and their default value. The environment variables are used in cloud-config template and
TF_VAR_* will override terraform variables defined in resources/common/common.tf. Copy the envs.sh.sample configuration file  as *envs.sh*. before build.

The complete configurations:

```
###############################
# Environments for the cluster.
###############################

export AWS_PROFILE=<undefined>
export AWS_REGION=us-west-2
export AWS_VM_TYPE=hvm
export CLUSTER_NAME=<undefine>
export COREOS_UPDATE_CHANNEL=beta
export ENABLE_REMOTE_VERSIONING=false

# Kubernetes API server DNS name
export KUBE_API_DNSNAME=<kube-api.example.com>

export SCRIPTS=../scripts
export SEC_PATH=../artifacts/secrets
export SSHKEY_DIR=${HOME}/.ssh

################
# Terraform vars
################

# Allow ssh from: comma separated list of cidr blocks
export TF_VAR_allow_ssh_cidr="0.0.0.0/32"

# Default domain for route53 zone
export TF_VAR_route53_zone=<mylab.example.com>

export TF_VAR_aws_region=${AWS_REGION}
export TF_VAR_cluster_name=${CLUSTER_NAME}

# Default auto scaling group parameters: you can override in each resource's own envs.sh.
export TF_VAR_cluster_min_size=1
export TF_VAR_cluster_max_size=5
export TF_VAR_cluster_desired_capacity=1
export TF_VAR_coreos_update_channel=${COREOS_UPDATE_CHANNEL}
export TF_VAR_instance_type=t2.medium

# Route53 internal zone point to vault and api-server internal ELB. Not to
# clash with kubernetes "cluster.local"
export TF_VAR_cluster_internal_zone="cluster.internal"
export TF_VAR_kube_api_dnsname=${KUBE_API_DNSNAME}

# Terraform remote state bucket name, defined as ${AWS_ACCOUNT}-${CLUSTER_NAME}-terraform
# in /resources/common/common.mk.
export TF_VAR_remote_state_bucket=${TF_REMOTE_STATE_BUCKET}

# Auto-unseal vault after built/reboot
export TF_VAR_vault_auto_unseal=true

# Vault CA
export TF_VAR_vault_ca={country = "US", province = "California", \
	organization = "IT Department", common_name = "${TF_VAR_route53_zone}"}

# Vault release: restart vault service if changed
export TF_VAR_vault_release=0.7.0

# Default vault root ca common name, for cert signing
export TF_VAR_vault_rootca_cn=vault.${TF_VAR_route53_zone}

# Default VPC prefix
export TF_VAR_vpc_prefix="10.240"
```
