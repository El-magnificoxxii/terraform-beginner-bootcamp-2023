# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:


This project is going to utilize semantic versioning for its tagging
[semver.org](https://semver.org/)

The general format:
**MAJOR.MINOR.PATCH**, eg. `1.0.1`

**MAJOR** version when you make incompatible API changes
**MINOR** version when you add functionality in a backward compatible manner
**PATCH** version when you make backward compatible bug fixes
Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

## Install the Terraform CLI

### Considerations for Linux Distributions

This project is built on ubuntu. please consider checking ypur linux distribution and change according to your distribution needs
[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)   

Example of checking OS version:

```
$ cat /etc/os-release
NAME="Ubuntu"
VERSION="20.04.1 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.1 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```




### Considerations with the Terraform CLI changes

The Terraform CLI installation instruction have  changed due to gpg keyring. so we referred to the latest install CLI instructions via Terraform documentation and change the scripting for install

[Install the Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


### Refactoring into bash scripts

while fixing the teraform gpg deprecation issues, we noticed the bash script step were a considerable amount more code. so we decided to create a bash script to install the terraform cli.

The bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)




- This will keep the gitpod task file ([.gitpod.yml](.gitpod.yml)) tidy
- This will allow us an easier time to debug and excute manually Terraform CLI install
- This will allow better portability for other projects that needs to install Terraform CLI


### Shebang Considerations

A shebang (pronounced sha-bang) tells the bash scripts what program that will interprete the script e.g `#!/bin/bash`

ChatGpt recommends that we use the format for bash `#!/usr/bin/env bash`

- for portability for different OS distribution
- will search for user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)

#### Executions Considerations

When executing the bash script, we can use the `./` shorthand notation to execute the bash script

e.g `./bin/install_terraform_cli`

if we are using a script in .gitpod.yml we need to point the script to a program to interprete it.

e.g `source ./bin/install_terraform_cli`


#### Linux Permissions Considerations

in order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode

```sh
chmod u+x ./bin/install_terraform_cli
```
alternatively

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod


### Github lifecycle (Before, Init, Command)

We need to be careful when using init because it will not rerun if we restart an existing workspace.


https://www.gitpod.io/docs/configure/workspaces/tasks


### Working Env Vars

#### env command

We can list all Environment variables (Env Vars) using the `env` command

We can filter specific env vars using grep e.g `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command 

```sh
HELLO='world' ./bin/print_message

```

within a bash we can set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world'
echo $HELLO

```

#### Printing Vars

We can print an env var using echo  e.g. `echo $HELLO`

#### Scoping of Env Vars

When you open new bash terminals in vscode it will not be aware of env vars that you have set in another window. 

if you want env var to persist across all future terminals that are open, you need to set env vars in your bash profile e.g `.bash_profile`


#### Persisting Env Vars in Gitpod

We can persist env var in gitpod by storing them in Gitpod Secrets Storage

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set en vars in the `.gitpod.yml` but this can only contain non sensitive env vars 

#### AWS CLI installation

AWS CLI is installed for this project via the bash scripts [`./bin/install_aws_cli`](./bin/install_aws_cli)



[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)



We can check if our AWS credentials is configured correctly by running the following AWS CLI command
AWS CLI command:
```sh
aws sts get-caller-identity
```

if it is successful, you should see a json payload return that looks like this:

```json
{
    "UserId": "AIDATJSE743VIULGMK7T5",
    "Account": "212345671210",
    "Arn": "arn:aws:iam::225514563210:user/terraform-beginner-bootcamp"
}
```

We will need to generate AWS CLI credentials from IAM user in orther to use the AWS CLI 

## Terraform Basics


### Terraform Registry
Teraform sources their providers and modules from the terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)


- **Providers** is an interface to APIs that will allow you to create resources in terraform.
- **Modules** are a way to make large amounts of terraform codes modular, portable and sharable 

[Rando Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)


#### Terraform Init

At a strat of a new terraform project we will run `terraform init` to download the binaries for the terarform providers that we will use in this project 


#### Terraform Plan

This will generate out a changeset, about the state of our infraastructure and what will be changed

We can output the changeset ie. "plan" to be passed to an apply, but often you can just ignore outputing


#### Terraform Apply

`terraform apply`

This will run a plan and pass the changeset to be executed by terraform. Apply should prompt YES or NO 

if we want to automatically approve an apply we can provide the auto approve flag e.g `terraform apply --auto-approve`


### Terraform Lock Files

`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used for this project

The Terraform lockfiles should be commited to your version control system e.g Github

### Terraform State Files
`.terraform.tfstate` contains information about the current state of your infratstucture. 

This file **should not be commited** to your version control system

This file can contain sensitive data

If you lose this file, you lose knowing the state of your infrastructure.

`.terraform.tfstate.backup` is the previous state file state.

### Teraform Directory

`.terraform` directory contains binaries of terraform providers 