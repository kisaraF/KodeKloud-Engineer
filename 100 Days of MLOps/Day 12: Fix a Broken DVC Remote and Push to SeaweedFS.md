# Day 12: Fix a Broken DVC Remote and Push to SeaweedFS

## Task Description

The xFusionCorp Industries ML team uses SeaweedFS as the shared S3-compatible object store for DVC-tracked data. A `.dvc/config` already declares a remote called `s3` for the fraud-detection project, but `dvc push` currently fails. Correct the configuration and push the tracked data into the SeaweedFS bucket.


A project exists at `/root/code/fraud-detection/` with DVC initialised and `data/raw/transactions.csv` already tracked.

SeaweedFS is already running on the controlplane:
- **S3 endpoint**: `http://localhost:8333`
- **Filer UI**: open the **SeaweedFS Filer** button at the top of the lab (forwarded port 8888) – buckets are visible under `/buckets/`.
- **Credentials**: `weedadmin` / `weedadmin123` (already set in `.dvc/config`)
- **Bucket name**: `dvc-storage` (already created and visible in the Filer UI under `/buckets/dvc-storage`)

Run `dvc push` to see it fail, then inspect `.dvc/config` against the endpoint, bucket, and credentials above.

Acceptance criteria:
- The remote called `s3` points at the `dvc-storage` bucket using `s3://`, uses the correct SeaweedFS S3 endpoint URL, and is marked as the default remote.
- After the push, the dvc-storage bucket in the SeaweedFS Filer UI contains at least one object under the `files/md5/...` prefix.

## Solution

**1. Run `dvc push` command**

This will state the issue. Issue will denote that the remote URL is not correctly configured

**2. Edit the configuration**

Use vi to edit the configuration at the `.dvc/config`. Add the following,

```
url = s3://dvc-storage
endpointurl = http://localhost:8333
```

The rest is okay. Then if you try a `dvc push` again. It will still give the same error.

**3. Setting default remote**

The default remote needs to be set with the following command

```bash
dvc remote default s3
```

The remote name can be found using the following

```bash
dvc remote list
```

Then the configuration will look like this

```
[core]
    remote = s3
['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```

Afterwards, you can push.