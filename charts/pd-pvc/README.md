# pd-pvc Helm Chart

This chart is specific to Google Cloud and GKE, and allows you to create a PersistentVolume and a PersistentVolumeClaim from an existing GCE Persistent Disk (PD).

## Usage

1. Create snapshot from GCE Persistent Disk:
    ```sh
    export PD=
    export SNAPSHOT=
    ```
    ```sh
    gcloud compute disks snapshot $PD \
    --zone europe-west1-d \
    --snapshot-names $SNAPSHOT
    ```
2. [Disaster that destroys PV / PVC / GCE PD]

3. Create new PD from snapshot:
    ```sh
    export NEW_PD=
    ```
    ```sh
    gcloud compute disks create $NEW_PD \
    --source-snapshot=$SNAPSHOT \
    --zone=europe-west1-d \
    --type=pd-standard
    ```
4. Add `pd-pvc` chart to your `requirements.yaml` of you application chart:
    ```yaml
    dependencies:
    - name: pd-pvc
      version: 0.1.0
      repository: https://exekube.github.io/charts/
      condition: pd-pvc.enabled
    ```
    Configure the subchart in `values.yaml`:
    ```yaml
    pd-pvc:
      enabled: true
      pvcName: my-new-pvc-from-snapshot-1
      gcePdName: $NEW_PD
    ```
5. Use `pvcName` in `postgresql.persistence.existingClaim`:
    ```yaml
    postgresql:
      persistence:
        enabled: true
        existingClaim: my-new-pvc-from-snapshot-1
    ```
