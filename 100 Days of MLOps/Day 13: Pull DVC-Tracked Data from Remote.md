# Day 13: Pull DVC-Tracked Data from Remote

## Task Description

A new team member at xFusionCorp Industries has cloned the fraud-detection repository onto a fresh machine. Although the DVC remote is correctly configured to point to the team's SeaweedFS bucket, the `dvc pull` command is failing. Your task is to diagnose the cause of this failure, correct the configuration as needed, and successfully pull the dataset.


A cloned project exists at `/root/code/fraud-detection/` with DVC initialised and the `data/raw/transactions.csv.dvc` pointer file present, but the dataset itself is missing from disk and from the local DVC cache.

SeaweedFS is already running on the controlplane and the dataset has already been pushed to the dvc-storage bucket — open the SeaweedFS Filer button at the top of the lab and navigate to `/buckets/dvc-storage/` to confirm the object is there.

- S3 endpoint: `http://localhost:8333`
- Credentials: `weedadmin` / `weedadmin123`

Run `dvc pull` to see it fail, then inspect `.dvc/config` against the endpoint and credentials above.

Acceptance criteria:

- The `s3` remote in `.dvc/config` reaches SeaweedFS with the access key (`access_key_id`) `weedadmin` and the secret key (`secret_access_key`) `weedadmin123`.
- After the pull, `data/raw/transactions.csv` is present on disk and its content matches the hash recorded in the `.dvc` pointer

## Answer

**1. Try running `dvc pull` first**

Diagnose will show that credentials are not set up correctly in the config file. When display the contents of the config file at `.dvc/config` it will only has the remote name, bucket name and forwarding URL only.

**2. Set up credentials**

Use the following commands to set up credentials to show in the config file as well

```bash
dvc remote modify s3 access_key_id weedadmin
dvc remote modify s3 secret_access_key weedadmin123
```

This will set up the credentials

**3. Pull dataset and verify metadata**

```bash
dvc pull
ll data/raw/ # check the contents of the directory
cat data/raw/dataset.csv.dvc # check metadata match or not
```

> At first only the .csv.dvc file will be there since it is the version controlled dataset in dvc.

## Reference

Read more about pulling data from remote in [this documentation](https://doc.dvc.org/user-guide/data-management/remote-storage/amazon-s3)