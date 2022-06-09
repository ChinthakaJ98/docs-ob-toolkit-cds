###Configuring MTLS Enforcement Executor

You can apply the `MTLSEnforcementExecutor` executor to check if a Mutual Transport Layer Security (MTLS) certificate is
present in the API request:

- The relevant configuration is in the `<APIM_HOME>/repository/conf/deployment.toml` file as follows:
```toml
[[open_banking.gateway.openbanking_gateway_executors.type.executors]]
name = "com.wso2.openbanking.accelerator.gateway.executor.impl.mtls.cert.validation.executor.MTLSEnforcementExecutor"
priority = 1
``` 

###Configuring certificate revocation validation

You can apply the `CertRevocationValidationExecutor` executor to perform the Online Certificate Status Protocol (OCSP) and
Certificate Revocation List (CRL) certificate revocation validation in the API request:

- The relevant configuration is in the `<APIM_HOME>/repository/conf/deployment.toml` file as follows:
```toml
[[open_banking.gateway.openbanking_gateway_executors.type.executors]]
name = "com.wso2.openbanking.accelerator.gateway.executor.impl.mtls.cert.validation.executor.CertRevocationValidationExecutor"
priority = 2
```

!!!tip
    By default, WSO2 Open Banking API Manager executes the certificate revocation validation. However, you can set a proxy
    and execute the certificate revocation validation. In that case, configure the proxy in `<APIM_HOME>/repository/conf/deployment.toml`
    as follows:
    ```toml
    [open_banking.gateway.certificate_management.certificate.revocation.proxy]
    enabled = true
    host = "PROXY_HOSTNAME"
    port = 8080
    ```

###Configuring external TPP validation

!!!note
    You need to enable either TPP validation or role validation as explained in this section. Otherwise, any kind of 
    TPP validation or role validation will not happen.

1. Open the `<APIM_HOME>/repository/conf/deployment.toml` file.

2. External TPP validation is enforced at the API level. Apply the `APITPPValidationExecutor` executor to compare the 
roles in the transport certificate against the roles in the request scope. If the bank needs TPP validation 
enabled, enable the following configurations:

    ```toml
    [open_banking.gateway.tpp_management.tpp_validation]
    enabled = true 
    ```
   
3. If a TPP validation is not configured, a TPP role validation will be performed. For this to happen, enable the following:

    ```toml
    [open_banking.gateway.tpp_management.psd2_role_validation]
    enabled = true
    ```

4. For TPP role validation, the applicable role names should be configured against the scope names as follows:
    - The sample configuration below performs role validation for AISP flow.

       ```toml
       [open_banking.gateway.tpp_management.psd2_role_validation]
       enabled = true
       [[open_banking.gateway.tpp_management.allowed_scopes]]
       name = "accounts"
       roles = "AISP"
       ```