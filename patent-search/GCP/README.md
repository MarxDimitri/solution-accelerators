# Patent Search on Google Cloud

## Prepare

- Prepare software dependencies

```bash
$ brew install jq
```

- Clone the repositoriy

- Create local env files

```bash
$ mkdir local_env
$ touch terraform.tfvars.json
```

- Modify the terraform env settings

```json
{
  "elastic_version" : "[PUT ELASTIC VERSION HERE]",
  "elastic_region" : "[PUT ELASTIC CLOUD REGION HERE]]",
  "google_cloud_project" : "[PUT GOOGLE CLOUD PROJECT NAME HERE]",
  "google_cloud_dataflow_job_name"  : "[PUT NAME OF DATAFLOW JOB HERE]",
  "google_cloud_region" : "[PUT GOOGLE CLOUD REGION FOR THE DATAFLOW JOB HERE]",
  "google_cloud_inputTableSpec" : "patents-public-data:patents.publications"
}
```

- Create Elastic Cloud ID following this steps.

- Set env variable for Elastic Cloud:

```bash
$ export EC_API_KEY="[PUT YOUR ELASTIC LOUD API KEY HERE]"
```

- Create Google Cloud service account following this steps.

- Create json for Google Cloud credentials. Follow the instractions here.

- Set env variable for Google Cloud credentials: 

```bash
$ export GOOGLE_CREDENTIALS="[PUT YOUR GOOGLE CLOUD CREDENTIALS JSON FILE HERE]"
```

- Set permission for the Google Cloud service account.

```bash
$ gcloud projects add-iam-policy-binding "[PUT YOUR GOOGLE CLOUD PROJECT NAME HERE]" \
--member=serviceAccount:[PUT YOUR SERVICE ACCOUNT MEMBER HERE] \
--role=roles/dataflow.worker

$ gcloud projects add-iam-policy-binding "[PUT YOUR GOOGLE CLOUD PROJECT NAME HERE]" \
--member=serviceAccount:[PUT YOUR SERVICE ACCOUNT MEMBER HERE] \
--role=roles/resourcemanager.projectIamAdmin

$ gcloud projects add-iam-policy-binding "[PUT YOUR GOOGLE CLOUD PROJECT NAME HERE]" \
--member=serviceAccount:[PUT YOUR SERVICE ACCOUNT MEMBER HERE] \
--role=roles/bigquery.admin
```

- Verify permissions
```bash
$ gcloud projects get-iam-policy "[PUT YOUR GOOGLE CLOUD PROJECT NAME HERE]" \
--flatten="bindings[].members" \
--format='table(bindings.role)' \
--filter="bindings.members:[PUT YOUR SERVICE ACCOUNT MEMBER HERE]"
```

### Deploy

- Initialize

```bash
$ terraform init
```

- Check plan

```bash
$ terraform plan -var-file="../local_env/terraform.tfvars.json"
```

- Run

```bash
$ terraform apply -var-file="../local_env/terraform.tfvars.json" -auto-approve
```

### Cleanup

```bash
$ terraform destroy -var-file="../local_env/terraform.tfvars.json" -auto-approve
```


<hr />
Dimitri Marx [dimitri at elastic.co] is an Partner Solutions Architect Lead in EMEA.