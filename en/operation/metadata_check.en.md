## Metadata Check

Usually we suggest you to backup metadata in any case. Now you can use metadata checker to recover medatada in some cases when metadata inconsistence happens weirdly.

Run metadata checker to identify isolated metadata:

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker check
```

If there are isolated metadata, run following command to recover the metadata ：

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker recovery
```

