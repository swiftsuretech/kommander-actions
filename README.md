# kommander-actions

A completely hands free deployment of dkp and / or kommander to a cloud provider

 - Clone the repo
 - Adjust the variables in the settings file to taste
 - Run the deploy file
 
### For AWS deployment

Ensure that maws is installed on your machine. The script will refresh your creds and push them to CAPI

### For AZURE deployment

Follow these instructions in order to generate a service principal:

https://github.com/kubernetes-sigs/cluster-api-provider-azure/blob/main/docs/book/src/topics/getting-started.md#prerequisites

- Carefully update the variables in "settings" for your env't when done
- Do not put your password here. Go to your Github repo / Settins / Secrets and add it there as AZURE_CLIENT_SECRET

Warnings!

- Do not put your password in the clear. It needs to be added as an "actions" secret in your repo
- Be careful to ensure you do not stage and push your kubeconfig file. That's even more dumb
