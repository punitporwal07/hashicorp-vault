# DEV Server
Start DEV server
```ps
vault server -dev
```

# Before login
Set vault address before log in
```ps
$env:VAULT_ADDR="http://127.0.0.1:8200"
```

# Login
## Login using token
```ps
vault login
```

## Login using userpass
```ps
vault login -method=userpass username=$userName
```

## Secret / KV
### Set a secret in KV Secret Engine
```ps
vault kv put $kvName/$secretName $key=$value
```

```ps
vault kv put secret/secretName key=value
// or old syntax
vault write secret/secretName key=value
```

### Get a secret from KV Secret Engine
```ps
vault kv get secret/secretName
// or old syntax
vault read secret/secretName
```

### Move a secret between KV Secret Engines
```ps
vault secrets move kvSecret1/secretName kvSecret2/secretName 
```

### List all secrets in a Secret Engine
```ps
vault kv list secretEngineName
```

### List all Secrets Engines
```ps
vault secrets list
```

### Upgrade Secret Engine to v2 (v1 is default)
```ps
vault kv enable-versioning secret
```
### Create new KV Secret Engine
```ps
vault secrets enable -path=devkv kv
```

## Enable transit secret engine
```ps
vault secrets enable transit
```

## create encryption key `autounseal`
```ps
vault write -f transit/keys/autoseal
```

# Token
## Get information about current token
```ps
vault token lookup
```

# Userpass
## Login
```ps
vault login -method=userpass username=$userName
```

## Add a user
```ps
vault write auth/userpass/users/arthur password=dent
```

## List users
```ps
vault list auth/userpass/users
```

## List accessors https://developer.hashicorp.com/vault/docs/concepts/tokens#token-accessors
```ps
vault list /auth/token/accessors
```

# Policies
## List all policies
```ps
vault policy list
```

## Create a policy using autounseal.hcl
```ps
vault policy write autounseal autounseal.hcl
```

# Snapshot
## Create automated snapshot
```ps
vault write /storage/raft/snapshot-auto/config/dev-backup interval="12h" retain=10 path_prefix="/data/vault_home/vault_files/snapshots/" file_prefix="vault-dev-snapshot" storage_type="local" local_max_space=50737418
```

## Read snapshot config
```ps
vault read /storage/raft/snapshot-auto/status/dev-backup
```
## list snapshot config
```ps
vault list /storage/raft/snapshot-auto/config
```

## delete snapshot config
```ps
vault delete /storage/raft/snapshot-auto/config/dev-backup
```

## Restore data from snapshot
```ps
vault operator init -key-shares=1 -key-threshold=1
vault operator raft snapshot restore -force vault-dev-snapshot-123456789657480786.snap
vault operator unseal vanBssslvadjypDEMOKEYGK1ZsT8FYW/qpS36iJ 	
vault operator unseal vanBssslvadjypChangeTheKeyGK1ZW/qpS36iJ 	
vault operator unseal vanBssslvadjypUnsealThreeTimesW/qpS36iJ
```


