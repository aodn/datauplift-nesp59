# Guidelines for general data processing flows

## Data 

### Describe the dataset.  

In a table (similar to what Ben has). Maybe create a clean template in excel to record the following information:

- Name of the dataset  
- Access point for download   
- Access method (API, HTTP server, manual, email, etc)   
- Point of contact: Name, Organisation, email address
- Minimum data requirements (more below)
- ...


### Download the data an explore it 

- Verify and tabluate contains the minimum required fields:
    - (lat/lon)/geometry + CRS
    - timestamp
    - species
    - species encoding (one hot encoded, array delimited)
    - species taxon mapping to WoRMS
 
- Implement ETL for the data source if the data source is valid:

    - Extract:
        - Input data validation
          
    - Transform:
        - Dates to `YYYY-MM-DD` (ISO 8601 date)
        - Map species to WoRMS
        - Project location data to EPSG:4326 (if required)
        - Model to fit Darwin Core
     
    - Load:
        - Save to a Darwin Core file

## Metadata 

Ideally we should collect all the required metadata info to be able to create a DwC metadata record (if occurrences) and/or a standard iso19115 record for the AODN Geoetwork 

- Extract, programmatically if possible (yes, in the case of a geoserver) the metadata records  
- Record the metadata into  a JSON file  

For the occurrence (DwC) data, create the corresponding EML (XML) file. Need a template for this with the mandatory fields. See the EML in the [OBIS manual](https://manual.obis.org/eml)

## flowchart

```mermaid
flowchart TD
    subgraph Datasets
        DS1[Dataset 1]
        DS2[Dataset 2]
        DS3[Dataset 3]
        DS4[Dataset ...]
    end
    subgraph Explore_the_Dataset
        A1[Identify Access Point]
        A11[Download the Data]
        A2[Document Schema]
        A3[Document Metadata]

        P1(JSON Schema)
        P2(XML file)
    end

    subgraph Transformation
        B1[Create DwC File]
        B2[Create EML File]
        B3[Create a CSV file]
    end
    
    Datasets --> Explore_the_Dataset
    A1 --> A11
    A11 --> A2
    A11 --> A3
    A2 --> P1
    A3 --> P2

    Explore_the_Dataset --> Transformation

    classDef lightGreen fill:#d4f4dd,stroke:#333,stroke-width:1px;
    class Datasets,Explore_the_Dataset,Transformation lightGreen;

    
```
