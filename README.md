# mermaid-ingest-docs

MERMAID ingestion API Insomnia collection and workflow documentation

## API Client

Insomnia is an API client used to make life easier when working with REST APIs and we'll be using this tool to help out with ingesting data into the MERMAID platform.  This repository includes an Insomina workspace import file (`mermaid_ingest_workspace.json`), which loads API calls for ingesting different Sample Unit type records from CSV files into Insomina.

Can be downloaded from https://insomnia.rest/


## Environments

### Sub Environments

Environments have been created for `local`, `dev` and `prod` environments.  The environment details needed to autenticate and make calls to the API have already been setup.


## Requests Available

* `Ingest Fish Belt Collect Records`
* `Ingest Benthic PIT Collect Records`


## Request Settings

### Multipart Tab

* `file`: Select file to upload
* `protocol`: Depending on the `Request`, this value has been preset to `benthicpit` or `fishbelt`
* `dryrun`:
  * if set to `true`, records are validated and valid collect records are created but NOT saved to the dastabase. 
  * if set to `false`, records are validated and valid collect records are saved to the database.
  
  

## How to Ingest Records

1. Set environment, `Use Local`, `Use Dev`, or `Use Prod`

2. Change `<PROJECT>` placeholder to project id in URL section, example:
  
  
  ```
    api_url/projects/<PROJECT>/collectrecords/ingest/
    
    # Change to...
  
    api_url/projects/4080679f-1145-4d13-8afb-c2f694004f97/collectrecords/ingest/
  ```
    
3. In the Mutlipart tab, select file to ingest in the `file` parameter s.
4. In the Mutlipart tab, set `true` or `false` in the `dryrun` parameter.
5. Click `Send`
