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