# MainFlux Vagrant Setup
#### Install Google Cloud SDK on your machine
Follow the instructions here: https://cloud.google.com/sdk/docs/downloads-interactive

Once the SDK is setup then please run the `gcloud init` command.
https://cloud.google.com/sdk/docs/initializing

Get your GCP Project ID:
```
$ gcloud projects list
PROJECT_ID                NAME                      PROJECT_NUMBER
mainfluxXXXX-XXXX       MainfluxTest              5XXXXXXXXX30
```

Create the secrets file with the ProjectID:
```
$ mkdir -p secrets
$ echo "mainfluxXXXX-XXXX" > secrets/gcp_project_id
```

Generate the service account key JSON file:
```
$ gcloud iam service-accounts list
NAME                                    EMAIL                                      DISABLED
Compute Engine default service account  XXX-compute@developer.gserviceaccount.com  False

$ gcloud iam service-accounts keys create --iam-account XXX-compute@developer.gserviceaccount.com secrets/gcp-service-account-key.json
```

#### Install Vagrant on your machine
Follow the instructions here: https://www.vagrantup.com/docs/installation/

Once the vagrant is setup now install the Google Plugin:
```
$ vagrant plugin install vagrant-google
```

##### Bootstrap your GCE VM
```
$ vagrant up --provider=google
```

If you're running into SSH issues then please add `--debug` flag to the `vagrant` command.
