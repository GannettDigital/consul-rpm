# Webhook workflow for consul rpm package
To be able to keep some kind of integration with public/forked repositories and our private resources and workflows, we created a workflow in this repository that will allow us to trigger builds on a private repository using repository dispatcher as a tool.

What it does is to enable manual run and request some inputs (parameters) which define the local target path for the created files and destination yum repository where the binary (or binaries) will be uploaded.
Also, we can decide if we want the file uploaded and/or the repository updated.

### Requirements
For all this to work we need to be able to reach to a private repository, passing parameters (payload) to it, and we won't be able to do it without a token.
This token is expected to be found between the secrets of this repo and will be called like **${{ secrets.ACTION_TOKEN }}** during the dispatch event.

### Inputs
* files_path: Where to find the local binaries.
* server: Destination yum repository.
* baseurl: Yum repository path destination.
* upload: Whether to upload the files or not.
* update: Whether to update the Yum repository index or not.

### Repository dispatch
It is actually a simple curl command, that contains:
token: to be able to access the private workflow.
the dispatch call to the target workflow: repository dispatcher event.
payload: the target branch and the inputs sent for former processing.

### Discovery
Public workflows don't have access to our private runners.
Secrets are updated by hand.
