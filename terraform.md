# Terraform Certification Prep

Basic commands

- init
- validate
    - syntax validation
- plan
- apply
    - -auto-approve
- destroy
    - -auto-approve
- refresh
    - update state file to aws actual
        - e.g infra config drift
    - plan also does a refresh

Variables

- string
- integer
- boolean
- list
    - type = list(string/integer/boolean)
    - single type only
- tuple
    - type = tuple([string, number, string])
- object
    - type = object({ name = string, port = list(number) })
- map

depends_on

- manually add dependency to a resource
- list of resources
- resource.name

version operator

- = ← exact
- != ←not equal
- >, >=, <, <=
- ~> ← pessimistic contraint operator
    - ~> 0.9 means
        - >= 0.9 & < 1.0
    - ~> 0.8.4 means
        - >= 0.8.4 & < 0.9

provider

- version
    - version = "2.7.0"
    - default is latest
- alias
- in resource set
    - provider = provider_name.alias_name
    - e.g aws.ireland
        - provider = aws
        - alias = ireland
- if plugin not available, add file manually in the directory
- local-exec
    - from local machine
- remote-exec
    - on the remote instance

Advance Commands

- fmt (format)
- taint resource_type.resource_name
    - taint will force recreate
- untaint resource_type.resource_name
- import resource_type.resource_name aws_id
    - use to import existing infra already deployed on aws
    - after import, it becomes under terraform control
- workspace list
    - show what workspaces are there
- workspace new workspace_name
- workspace show
- workspace select workspace_name
- workspace delete workspace_name
- state list
    - shows all resources inside the state file
- state mv resource_type.resource_old_name resource_type.resource_new_name backup-out=filebackup
    - rename the resource
- state pull
    - pull from remote state
    - see what is in the remote state
- state push
    - force local to remote
- state rm resource_type.resource_name
    - delete a resource from state file
    - e.g removed resource via console
- force-unlock lock_id
    - very dangerous, last resort
    - state locking, use dynamodb

Importance of state file

- keep track of what has been created
- secrets are stored in state encrypt at rest
- state locking when working w teams
- 

How to secure keys

- variables
- aws cli
- vault

Variable Types

- string
- number
- boolean
- list
    - can only be string, number or boolean
- tuple
    - a list with different type of objects
- object
    - a predefined json like object with specific keys

Outputs

- Syntax: resource_type.resource_name.attribute

Dependency when creating resources

- can use when something must be created before something else
- Syntax: depends_on = [resource_type.resource_name]

Setting variables

- via environment

    export TF_VAR_variablename=value

- via CLI

    terraform plan -var="variablename=value"

- via .tfvars file
    - terraform plan -var-file=prod.tfvars
    - terraform plan -var-file=prod.tfvars.json

Variable loading order

1. -var or -var-file option
2. Any.auto.tfvars or .auto.tfvars.json
3. Terraform.tfvars.json
4. Terraform.tfvars
5. environment variables

Debugging

- not meant for end users to use
- useful when terraform itself creates errors and need to submit log file to hashicorp
- TRACE is the most verbose level
- export TF_LOG=TRACE

State

- state locking, when there are multiple ppl working on the same state file

Backend

- backend.tf
- revert back to local by removing the backend config
- cannot have variables or string interpolation
- remote backend limitations
    - only 1 remote backend allowed
    - security
        - encrypting at rest, state file will contain secrets
        - state pulled down into memory only

terraform cloud

- encrypt state file at rest

oss vs cloud

- configuration
    - Local: on disk
    - Cloud: linked repo
- variable values
    - local: .tfvars, CLI argv or shell env
    - cloud: in workspace
- state
    - local: on disk or remote backend
    - cloud: in workspace
- credentials & secrets
    - local: shell env or enter on prompt
    - cloud: in workspace, stored as sensitive variables

cloud vs enterprise

- enterprise
    - hosted version
- cloud
    - non-hosted