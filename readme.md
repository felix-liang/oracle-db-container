# Install Oracle DB on OpenShift Container environment

The helm chart utilize the free docker images from Oracle Container Registory.
Please check the `https://container-registry.oracle.com/ords/ocr/ba/database` for more details.


## Configurations

Please check and modify the configuration in `helm/oracledb/values.yaml`.

## Run Helm Chart

```
helm install myoracledb  helm/oracledb
```

It take several minutes to initlize the Oracle DB instance.

## Set up SSL connection for Oracle

If you want to use SSL connection, please refer to the `setup-oracle-ssl-conn.md`.

* The CA certificate and truststore can be found under `ssl` folder.
