# Pegen

> Person Generator

## Azure functions

https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=macos%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-typescript

```bash
asdf install azure-functions-core-tools 4.0.5611
asdf reshim azure-functions-core-tools
func init pegen --worker-runtime typescript --model V4
func new --template "Http Trigger" --name generate
```

##  Publish
```bash
az login
func azure functionapp publish fa-nielsd
```

### Issues on publishing

#### Executable 
```
An error occurred trying to start process '/Users/niedel/.asdf/installs/azure-functions-core-tools/4.0.5611/gozip' with working directory '/Users/niedel/private-repos/pegen'. Operation not permitted
```

Solved by making it executable:
```
niedel@mbp pegen % cd /Users/niedel/.asdf/installs/azure-functions-core-tools/4.0.5611/
niedel@mbp 4.0.5611 % ls -l gozip
        -rw-r--r--  1 niedel  staff  2472512 Apr  3  2024 gozip
niedel@mbp 4.0.5611 % chmod +x gozip
```

#### Function not visible in Azure after deploy

```
niedel@mbp pegen % func azure functionapp publish fa-nielsd
Getting site publishing info...
[2024-10-16T19:49:31.781Z] Starting the function app deployment...
Creating archive for current directory...
Uploading 1,75 MB [###############################################################################]
Upload completed successfully.
Deployment completed successfully.
[2024-10-16T19:49:47.537Z] Syncing triggers...
Functions in fa-nielsd:
```

```
niedel@mbp pegen % az functionapp function list -g rg-nielsd -n fa-nielsd
[]
```

After a 3rd attempt:
```
niedel@mbp pegen % func azure functionapp publish fa-nielsd
Getting site publishing info...
[2024-10-16T20:17:17.612Z] Starting the function app deployment...
Creating archive for current directory...
Uploading 1,75 MB [###############################################################################]
Upload completed successfully.
Deployment completed successfully.
[2024-10-16T20:17:33.423Z] Syncing triggers...
Functions in fa-nielsd:
    generate - [httpTrigger]
        Invoke url: https://fa-nielsd.azurewebsites.net/api/generate

```
