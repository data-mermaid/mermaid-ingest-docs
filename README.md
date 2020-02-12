# mermaid-ingest-docs

MERMAID ingestion API Insomnia collection and workflow documentation

## API Client

Insomnia is an API client used to work with RESTful APIs, and we'll be using this tool in this repository to ingest data
 into the MERMAID platform. This repository includes an Insomina workspace import file 
 (`mermaid_ingest_workspace.json`), which handles authentication and loads API calls for ingesting different Sample Unit
  type records from CSV files.

Insominia can be downloaded from https://insomnia.rest/


## Environments

Environments have been created within the Insomnia workspace for

`local`: [http://localhost:8888](http://localhost:8888): Used for software development testing  
`dev`: [https://dev.collect.datamermaid.org](https://dev.collect.datamermaid.org): Used for Quality Assurance testing
 of new features, bug fixes, etc.  
`prod`: [https://collect.datamermaid.org](https://collect.datamermaid.org): Live application that everyone uses.

Also, the details needed to authenticate and make calls to the MERMAID API have been set up.


## Ingests Available

* `Ingest Fish Belt Collect Records`
* `Ingest Benthic PIT Collect Records`


## Ingest Settings

### Multipart Tab

* `file`: Select file to upload
* `protocol`: Depending on the `Request`, this value has been preset to `benthicpit` or `fishbelt`
* `dryrun`:
  * if set to `true`, records are validated and valid collect records are created but NOT saved to the dastabase. 
  * if set to `false`, records are validated and valid collect records are saved to the database.
  

## What do you have to do before ingesting records?

If you don't have a project to ingest your records into, go to [MERMAID Collect](https://collect.datamermaid.org
/) (or [MERMAID Collect dev](https://dev-collect.datamermaid.org/) for dev, etc.) to create a new project. Your project
 must include all Sites, Mangement Regimes and Observers that are used in your CSV data. Sites and mangement regimes'
 names match the names used in the CSV file, and the emails of the observers match the observer emails used in the
  CSV file. Below is an illustration of a CSV file and site, management regimes and observer records that have been
  added to a project using the Collect application.

![Setup](/assets/setup.png)


## How to ingest records?

1. Set environment: `Use Local`, `Use Dev`, or `Use Prod`

2. Change `<PROJECT>` placeholder that can be found in the URL section of Insomina to the id of the project that you
 will be ingesting records into. 

Example:  
  
  ```
    api_url/projects/<PROJECT>/collectrecords/ingest/
    
    # Change to...
  
    api_url/projects/4080679f-1145-4d13-8afb-c2f694004f97/collectrecords/ingest/
  ```
    
3. In the Mutlipart tab, next to the `file` parameter, select CSV file.
4. In the Mutlipart tab, next to the `dryrun` parameter, set `true` or `false`. ([What??](#multipart-tab))
5. Click `Send`


[![HowTo](/assets/video.png)](https://www.loom.com/share/5e16903be1d346faa12d1ff3692e9917)


6.

If ingest was **successful** ([200 OK](#valid-ingest)), you will see JSON-formatted results in the righthand
 Insomnia pane, and your records will be available to view in the [Collect
 application](https://collect.datamermaid.org/).

#### Valid Ingest

![Valid](/assets/valid.png)


If ingest was **unsuccessful** ([400 Bad Request](#invalid-ingest)), you will recieve a response in the righthand
 Insomnia pane listing out validation errors and where to find them in the CSV file. 
 [Interpreting unsuccessful ingest](#interpreting-unsuccessful-ingest) has more details on where to find your invalid
  records.

#### Invalid Ingest

![Invalid](/assets/invalid.png)


**Other ingest failure types**

* `500 Internal Server Error`: There can be many reasons why you get this error but one notable one is using a CSV
 file that does not follow the correct column naming (see [Ingest template files](#ingest-template-files)).


**Ingested records can now be validated and submitted in the [Collect application](https://collect.datamermaid.org/).**


7. Navigate to [Collect application](https://collect.datamermaid.org/#/projects)
8. Click on the project with the new ingested records.
9. New records are now available to go through the Validate and Submit workflow. 
Details can be found [here](https://datamermaid.org/docs/documentation/mermaid-workflow/validate-and-submit-data/)


## Interpreting ingestion errors

```
[
  {
    "Observer emails *": {                            <---- Column name in CSV file.
      "description": [
        "dustin@spageo.com doesn't exist"             <---- Description of why it is invalid.
      ]
    },
    "$row_number": 2                                  <---- Row in CSV file.
  },
  {
    "Fish name *": {
      "description": [
        "This field may not be null."
      ]
    },
    "$row_number": 12
  }
]
```

## Ingest template files

Ingest files follow a strict column naming schema. To make life a bit easier, you can find templates for the
 supported ingest Sample Unit Types in this respository.

* Fish Belt: [fishbelt_template.csv](/schemas/fishbelt_template.csv)
* Benthic PIT: [benthic_pit_template.csv](/schemas/benthic_pit_template.csv)

**NOTE: In each template, column names that end with `*` are required.**
