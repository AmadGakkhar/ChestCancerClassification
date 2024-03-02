# ChestCancerClassification

### Step 1. Create Project Template
Barebones folder structure using template.py

### Step 2. Add requirements.txt and create setup.py

### Step 3. Setup Logger in src/ChestCancerClassifier/__init__.py

### Step 4. Setup utils/common.py
Common utility functions.

### Step 5. Design Workflow

Design workflow for each stage of the project i.e, Data Ingestion, Training, Validation, Deployment etc.

    1. Update config.yaml
    2. Update secrets.yaml [Optional]
    3. Update params.yaml
    4. Update the entity
    5. Update the configuration manager in src config
    6. Update the components
    7. Update the pipeline
    8. Update the main.py
    9. Update the dvc.yaml

## Data Ingestion

    1. Update config.yaml

        data_ingestion:
            root_dir : artifacts/data_ingestion
            source_url : https://drive.google.com/file/d/1X1nl4bIAaCAitivNR2l0PT5-8HCz46Yy/view?usp=sharing
            local_data_file : artifacts/data_ingestion/data.zip
            unzip_dir : ardtifacts/data_ingestion

    2. Update secrets.yaml [Optional]

        No Secrets

    3. Update params.yaml

        No Params for Data Ingestion

    4. Update the entity

        Return type of config file information. Created using dataclass

        @dataclass(frozen=True)
        class DataIngestionConfig:
            root_dir : Path
            source_url : str
            local_data_file : Path
            unzip_dir : Path

    5. Update the configuration manager in src/config

        Create DataConfigurationManager Class and add get_data_ingestion_config method which returns DataIngestionConfig type object.

    6. Update the components

        

    7. Update the pipeline
    8. Update the main.py
    9. Update the dvc.yaml
