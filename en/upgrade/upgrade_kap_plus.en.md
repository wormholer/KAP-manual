## Upgrade to KAP Plus KyStorage Engine

KAP Plus introduced new storage engine: KyStorage. It is compatible with the traditional storage engine HBase used by Apache Kylin and KAP. The new cube will be built by KyStorage automatically. 

### Hybrid Upgrade

If the exiting Cube data should be reserved, and build new cube by KyStorage only, the Hybrid Cube will be used.

Take an example,  projecct name *learn_kylin*, model name *kylin_sales_model*, cube name *kylin_sales_cube*. Please follow the instructions:
1. In KAP Plus, clone *kylin_sales_cube*  to *kylin_sales_cube_plus*, the new cloned cube will use KyStorage by default.

2. Build new Cube, *kylin_sales_cube_plus*. Attention: No time range overlap between old and new cubes is allowed. 

3. Create new Hybrid Cube, to merge the above two Cube by metadata.

        bin/kylin.sh org.apache.kylin.tool.HybridCubeCLI -action create -name kylin_sales_hybrid -project learn_kylin -model kylin_sales_model -cubes kylin_sales_cube,kylin_sales_cube_plus

   If the build success, the log would show ```HybridInstance was created at: /hybrid/kylin_sales_hybrid.json```

4. Reload KAP metadata 

5. The sequent build will be based on *kylin_sales_cube_plus*, the old *kylin_sales_cube* should not build any new segment.



### Overwrite Upgrade

If the exiting Cube could be rebuilt, or the exiting Cube is not built by incremental way. Then we could drop the exiting Cube and rebuild the new Cube by KyStorage. 

Follow the steps to upgrade:

1. run `<KYLIN_HOME>/bin/metastore.sh backup --noSeg`. This command back up the whole metadata to `meta_backups` directory.
2. run `<KYLIN_HOME>/bin/metastore.sh promote meta_backups` . This command upgrade metadata to fit KyStorage.
3. run `<KYLIN_HOME>/bin/metastore.sh restore meta_backups`. This command restore the updated metadata.
4. Disable and purge the cubes and rebuild them.


