## Metadata Check

Ussally we suggest users to backup metadata, in order to recovery quickly. However, KAP is still blocked by metadata inconsistence and there are no available metadata backup.  In this case, try to use metadata check to resolve the issues.

Run metadata check：

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker check
```

If there are indeed some inconsistent metadata, run metadata recovery ：

```
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker recovery
```

