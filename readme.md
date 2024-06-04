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



## Create User in Oracle

* The default password for `sys` is `Passw0rd` *

- Login to the Oracle pod console
- Run the scripts below.

```
sqlplus sys/Passw0rd as sysdba;
ALTER SESSION SET CONTAINER = XEPDB1;
CREATE USER SI_USER IDENTIFIED BY password;

GRANT "CONNECT" TO SI_USER;
ALTER USER SI_USER DEFAULT ROLE "CONNECT";
GRANT CREATE SEQUENCE TO SI_USER;
GRANT CREATE ANY TABLE TO SI_USER;
GRANT CREATE TRIGGER TO SI_USER;
GRANT SELECT ON CTXSYS.CTX_USER_INDEXES TO SI_USER;
GRANT SELECT ON SYS.DBA_DATA_FILES TO SI_USER;
GRANT SELECT ON SYS.DBA_FREE_SPACE TO SI_USER;
GRANT SELECT ON SYS.DBA_USERS TO SI_USER;
GRANT SELECT ON SYS.V_$PARAMETER TO SI_USER;
GRANT SELECT ANY DICTIONARY TO SI_USER;
GRANT ALTER SESSION TO SI_USER;
GRANT CREATE SESSION TO SI_USER;
GRANT CREATE VIEW TO SI_USER;
ALTER USER SI_USER QUOTA UNLIMITED ON USERS;
```