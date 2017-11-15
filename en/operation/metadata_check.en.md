## Metadata Check

We suggest you backup the metadata regularly so that you may recover it quickly when corrupted. Nevertheless, there are still some unexpected accidents which cause the metadata inconsistent and KAP out of service. Fortunately, now you can use the metadata checker in KAP to recover the medatada when the metadata inconsistence happens weirdly.

### Check Scope

We summarize some scenarios which might cause the metadata inconsistent in KAP. They are as below:

1. cube against model (consistent check of cube and model)
2. cube against table index
3. cube against scheduler job
4. job's metadata against output information (i.e. consistent check of job itself)
5. KAP runtime loaded metadata against resource store's metadata

### Usage
Run the metadata checker to identify inconsistent metadata:

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker check
```

If there is any inconsistent metadata detected, run the following command to recover the metadata:

> Notes: this step will delete the isolated metadata detected in KAP.

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker recovery
```

