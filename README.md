# concource-ci-example

1. Read getting started guide on concourse ci website - https://concourse-ci.org/getting-started.html

2. Get a sample pipeline to work other than the "Hello World Pipeline" example.

3. Use inputs and outputs to exchange artifacts betweeen tasks.

4. Try Vault integration to read secrets from Vault. https://concourse-ci.org/vault-credential-manager.html

## How to vault
Download vault binary.

Use the vault config in this repo to get a vault server running.
```vault server -config=vault.hcl```

Login to http://vault-ip:8200/ui 
and unseal vault using the GUI using 1 key shares and threshold.
Save the file.

Follow the guide on vault credential manager section `Configuring the secrets engine`

Use the `approle` auth backend to auth to vault.

Add these three lines to your concourse docker-compose file:
```
CONCOURSE_VAULT_AUTH_BACKEND: "approle"
CONCOURSE_VAULT_AUTH_PARAM: "role_id:xxxxxxxxxxxxx,secret_id:xxxxxxxxxxxx"
CONCOURSE_VAULT_URL: http://192.168.1.10:8200
```

Create a KV v2 secret engine with the path concourse/main/gcp. Use the vault GUI.

Add a dummy secret named `demo` under `concourse/main/gcp` with some key and value.

Reference the secret you created in your `job.yml` using the syntax `((gcp.demo))`.

Try adding GCP OAUTH token on JSON key in vault and use it in your pipeline.



