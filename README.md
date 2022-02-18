# kommander-actions

## An (almost) completely hands-free deployment of dkp and / or kommander to either Azure or AWS.

This project is designed to demonstrate a way of deploying dkp, with or without Kommander, to a cloud provider without the need for a CLI. It is a proof of concept only and not production ready.

Github Actions may be set to trigger on a pre-determined event, such as a push to a defined branch or a pull request. Actions are defined in a yaml file located in the .github/workflows directory. The jobs execute on a "runner"; a VM hosted by Github, but it is also fairly simple to host your own runner if your requirements are more advanced.

## Overview

In our case, the workflow spins up an Ubuntu 20 virtual machine, checks out our repo, moves our dkp and Kommander binaries and our deploy script into the env't and manages our secret environmental variables, ie access tokens, that are stored encrypted as secrets in the repo. To amend, update or set our secrets, navigate to the repo / settings / Secrets / Actions in Github.  

The main deployment script, "deploy", is found in the root of the repo. Configuration file(s) are placed into the "config.d" directory and read by the script. It is therefore possible to use that script as a standalone rapid deployment tool from your local machine but you will need to place the secrets within the config.d directory which is sourced by the script.

## Security

Unless using the tool standalone as described above, it is important that there is no sensitive information stored in repos for security reasons. On the subject of security, it is not recommended to use a public repo for this type of activity to prevent users pushing potentially malicious code into the codebase.

## Usage

- Log into your Github account and navigate to [this repo](https://github.com/swiftsuretech/kommander-actions)

- Fork the repo (click "Fork" button on the top right) - this is important as you will need push privileges

- You may then either clone your repo to a local machine or edit directly in the Github UI:

### To work on a local machine

- Clone the repo

      git clone https://github.com/[my_repo]/kommander-actions
 
- Navigate to the setting directory
 
      cd kommander-actions/config.d
      
- Edit the settings file

      vim settings
      
- Adjust to taste to define cloud provider, whether you want a Kommander deployment on top, preferred region, number of control planes and worker nodes etc

- Provide your `non confidential` account data (see below for further info) by amending the environmental variables

- Provide your secret account data in the Github secrets store by navigating to Settings / Secrets / Actions in the Github UI. Ensure you follow the guide to ensure they are named correctly for your env't

- Set your cluster name `to a unique value` and save the config

- Navigate back to the root dir

      cd ../../
      
- Stage the updated settings file

      git add ./config.d/settings
      
- Set commit message to the name of the cluster

      git commit -m "my-azure-cluster"
      
- Push your changes

      git push
      
### To work in Github

- Log into your Github account

- Navigate to the settings file in config.d/settings

- Update as required for your deployment. `Don't forget to set a unique cluster name`

- Add a commit message in the UI. The name of the cluster is recommended.

- Click on "Commit changes"


## Authentication

### For AWS deployment

Ensure that maws is installed on your machine. The script will automatically refresh your creds and push them to CAPI. For a non D2 scenario, if you wish to use a certificate or  access token, amend the settings such that all non-confidential environmental variables are set in that script and that all secrets are added to Github secrets. You will then need to modify ./.github/workflows/main.yml to ensure that the correct secrets are extracted at runtime.

### For AZURE deployment

Follow these instructions in order to generate a service principal:

https://github.com/kubernetes-sigs/cluster-api-provider-azure/blob/main/docs/book/src/topics/getting-started.md#prerequisites

- Carefully update the variables in "settings" for your env't when done

- Do not put your password here. Go to your Github repo / Settings / Secrets and add it there as "AZURE_CLIENT_SECRET"

## Outcome

Observe progress by going to Actions in the Github UI. From there you may observe output from the script.

Once complete you will see the publically accessible URL, some login credentials and your kubeconfig. It is highly recommended to copy the kubeconfig output to your local machine for any onward troubleshooting or advanced access.

Actions output is only viewable to a correctly authenticated owner of that Github account. It is however highly recommended to make the repo private.
