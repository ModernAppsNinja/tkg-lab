# Install Velero and Setup Nightly Backup

## Install Velero client

Download and install the Velero cli from TKG 1.2.1 at https://www.vmware.com/go/get-tkg.

## Setup AWS as a target for backups

Follow [Velero Plugins for AWS Guide](https://github.com/vmware-tanzu/velero-plugin-for-aws#setup).  I chose **Option 1** for **Set Permissions for Velero Step**.

Store your credentials-velero file in `keys/` directory.

Go to AWS console S3 service and create a bucket for cluster backups.

>Note: Do this step regardless if you are deploying TKG to vSphere or AWS.

## Set configuration parameters

The scripts to prepare the YAML to deploy velero depend on a parameters to be set.  Ensure the following are set in `params.yaml` based upon your environment:

```yaml
velero.bucket: my-bucket
veloreo.region: us-east-2
```

## Prepare Manifests and Deploy Velero

Prepare the YAML manifests for the related velero K8S objects and then run the following script to install velero and configure a nightly backup.

```bash
./scripts/velero.sh $(yq r $PARAMS_YAML management-cluster.name)
```

## Validation Step

Ensure schedule is created and the first backup is starting

```bash
velero schedule get
velero backup get | grep daily
```

## Go to Next Step

Now management cluster steps are complete, on to the workload cluster.

[Create new Workload Cluster](../workload-cluster/01_install_tkg_and_components_wlc.md)