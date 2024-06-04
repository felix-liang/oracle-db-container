# Setup Oracle SSL configrations

## Prepare the certificates (Optional - Predefined certs are under ssl)

### Generate CA Private Key

```
openssl genpkey -algorithm RSA -out ca.key -aes256 -pass pass:Pass2341d!
```
### Create ca's selfsigned cert

```
openssl req -new -x509 -key ca.key -sha256 -days 3650 -out ca.crt -passin pass:Pass2341d!
```

### Generate Server's Private Key

```
openssl genpkey -algorithm RSA -out server.key -aes256 -pass pass:Pass98761d!
```

### Create Server's CSR:
```
openssl req -new -key server.key -out server.csr -passin pass:Pass98761d!
```


### Sign Server's Certificate with CA
```
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 3650 -sha256 -extensions v3_req -passin  pass:Pass2341d!
```


## Set up Oracle Wallet for SSL


### Create Oracle Wallet

```
orapki wallet create -wallet /opt/oracle/oradata/dbconfig/XE/wallet  -auto_login -pwd YourWalletPassword1!
```

### Add CA Cert to the wallet

```
orapki wallet add -wallet /opt/oracle/oradata/dbconfig/XE/wallet -trusted_cert -cert ca.crt -pwd YourWalletPassword1!
```

### Import server private key

```
orapki wallet import_private_key -wallet /opt/oracle/oradata/dbconfig/XE/wallet -pvtkeyfile server.key -cert server.crt  -pwd YourWalletPassword1!
```

The password of the key will be asked, please check the password from above step `Generate Server's Private Key`.



## Update the sqlnet.ora and listener.ora
```
oc cp conf/sqlnet.ora myoracledb-oracle-db-0:/opt/oracle/oradata/dbconfig/XE/sqlnet.ora
oc cp conf/listener.ora myoracledb-oracle-db-0:/opt/oracle/oradata/dbconfig/XE/listener.ora

```

## Restart Oracle

```
lsnrctl stop
lsnrctl start

```