- [Working with Azure Storage Fileshare using Azure Cli](#working-with-azure-storage-fileshare-using-azure-cli)
    - [Pre-requisite file share details](#pre-requisite-file-share-details)
    - [Add certificate chain](#add-certificate-chain)
      - [Download cert](#download-cert)
    - [get sas token](#get-sas-token)
    - [Create directory(date+month+year-randomnumber)](#create-directorydatemonthyear-randomnumber)
    - [Upload single file](#upload-single-file)
    - [Upload batch](#upload-batch)
    - [Folder batch download](#folder-batch-download)
  
# Working with Azure Storage Fileshare using Azure Cli

### Pre-requisite file share details

### Add certificate chain

File share certificate chain needs to be added in azure cli python site-package cacert.pem file. 

Reference article https://docs.microsoft.com/en-us/cli/azure/use-cli-effectively?view=azure-cli-latest#work-behind-a-proxy


#### Download cert

Download the certificate chain for given fileshare url i.e. https://o360deliverytfstatesbx.file.core.windows.net/pulsar-perf

Above has four certs, one root `Optum Root CA`, two intermediate `OptumInternalPolicyCA2`, `UHGInterwebproxy2020.uhc.com` and one azure file share `*.file.core.windows.net`

Convert `.cer` to `.pem`
https://support.ssl.com/Knowledgebase/Article/View/19/0/der-vs-crt-vs-cer-vs-pem-certificates-and-how-to-convert-them

python cacert.pem file location in busybox distro </br>
`/usr/local/lib/python3.6/site-packages/certifi`

azure cli python cacert.pem file location in mac </br>
`/usr/local/Cellar/azure-cli/2.10.1/libexec/lib/python3.8/site-packages/certifi`

Standard practice is to add cert in order, first add end user cert to root cert should be last.

### get sas token

`az login` is required before creating `sas` token

date on azurecli busybox
`expiryDate=$(date -d "+24:00:00" +%Y-%m-%dT%H:%MZ)`

date on mac os
`expiryDate=$(date -v+1d  +%Y-%m-%dT%H:%MZ)`

```bash
sas=`az storage share generate-sas -n pulsar-perf --account-name o360deliverytfstatesbx --https-only --permissions dlrw --expiry $expiryDate -o tsv`
```

### Create directory(date+month+year-randomnumber)

using sas_token - working command </br>
```bash
az storage directory create  \
--name $(date +%Y-%m-%d-$RANDOM) \
--share-name pulsar-perf \
--account-name o360deliverytfstatesbx \
--sas-token $sas \
--output none
```

### Upload single file

`az storage file upload -s two --account-name pulsarperfdemosa --source custom_pulsar-effectively-once.yaml --path "8-10-2020-12322ad"  --sas-token $sas`

az storage file upload -s pulsar-perf --account-name o360deliverytfstatesbx  --source cert/first.pem --path 2020-08-25-6199/cert --account-key f0IlWg3SWF4r7tFo4/Ftwr7DIYfn/5HjqRG2ZklRI/PfEtmnY8t2p46fcnUelYph6/nDExehui/VoeOVs0x0xg==

### Upload batch

Upload a local directory recursive into fileshare dir

`az storage file upload-batch --account-name pulsarperfdemosa --destination filesharename  --destination-path "test2" --source .  --sas-token $sas`

2020-21-31-52147
`az storage file upload-batch --account-name pulsarstandardrahul --destination pulsar-perf  --destination-path "2020-21-31-52147" --source .  --account-key ExeVWMyKzkNLP4SiwPzLa4ST70D3mY5FGzSor8QO55EP5D3kZRXsA0Te/fS+Alh/WOBiajJtpJ0zVkBykKdZaA==`

### Folder batch download

Below will  download entire fileshare `two`
`az storage file download-batch --account-name pulsarperfdemosa --destination .  --source two  --sas-token $sas`

Download all files from a directory  `test2`  from `two` fileshare
`az storage file download-batch --account-name pulsarperfdemosa --destination .  --source two/test2  --sas-token $sas`

We can also download files from nested directories using source path as `fileShareName/dirName/NestedDirName` i.e. `two/test2/result`


Check Container State

`az container show --name pulsar-perf-demoa  --resource-group pulsar-dev --query containers[0].instanceView.currentState.state`