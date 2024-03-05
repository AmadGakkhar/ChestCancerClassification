[![workflow](https://github.com/AmadGakkhar/ChestCancerClassification/actions/workflows/main.yaml/badge.svg)](https://github.com/AmadGakkhar/ChestCancerClassification/actions/workflows/main.yaml) 

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
            unzip_dir : artifacts/data_ingestion

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

        Create ConfigurationManager Class and add get_data_ingestion_config method which returns DataIngestionConfig type object.

    6. Update the components

        We will create a DataIngestion Class in a components file which will take DataIngestionConfig type object as argument the methods to perform the following tasks:

            Download Data from the URL
            Extract the Downloaded file to the path in config file.


    7. Update the pipeline

        Create a pipeline class for Data Ingestion stage which integrates the complete data ingestion process.

    8. Update the main.py

        In the main file, create an object of the class created in 7(Update pipeline) and call its main function. 

    9. Update the dvc.yaml


## Prepare Base Model

    1. Update config.yaml

        prepare_base_model:
            root_dir : artifacts/prepare_base_model
            base_model_path : artifacts/prepare_base_model/base_model.h5
            updated_model_path : artifacts/prepare_base_model/base_model_updated.h5

    2. Update secrets.yaml [Optional]

        No Secrets

    3. Update params.yaml

        AUGMENTATION: True
        IMAGE_SIZE: [224, 224, 3] # as per VGG 16 model
        BATCH_SIZE: 16
        INCLUDE_TOP: False
        EPOCHS: 1
        CLASSES: 2
        WEIGHTS: imagenet
        LEARNING_RATE: 0.01

    4. Update the entity

        Return type of config/param file information for Base Model Preparation. Created using dataclass

        @dataclass(frozen=True)
        class PrepareBaseModelConfig:
            root_dir: Path
            base_model_path: Path
            updated_base_model_path: Path
            params_image_size: list
            params_learning_rate: float
            params_include_top: bool
            params_weights: str
            params_classes: int

    5. Update the configuration manager in src/config

        Update ConfigurationManager Class and add get_prepare_base_model_config method which returns PrepareBaseModelConfig type object.

    6. Update the components

        We will create a PrepareBaseModel Class in a components file which will take PrepareBaseModelClassConfig type object as argument and have the methods to perform the following tasks:

            Get base model from Keras
            Save the Model
            Updates the Model for Transfer Learning


    7. Update the pipeline

        Create a pipeline class for Model Preparation stage which integrates the whole process.

    8. Update the main.py

        In the main file, create an object of the class created in 7(Update pipeline) and call its main function. 

    9. Update the dvc.yaml


## Model Training

    1. Update config.yaml

        training:
        root_dir: artifacts/training
        trained_model_path: artifacts/training/model.h5

    2. Update secrets.yaml [Optional]

        No Secrets

    3. Update params.yaml

        AUGMENTATION: True
        IMAGE_SIZE: [224, 224, 3] # as per VGG 16 model
        BATCH_SIZE: 16
        INCLUDE_TOP: False
        EPOCHS: 1
        CLASSES: 2
        WEIGHTS: imagenet
        LEARNING_RATE: 0.01

    4. Update the entity

        Return type of config/param file information for Training. Created using dataclass


        @dataclass(frozen=True)
        class TrainingConfig:
            root_dir: Path
            trained_model_path: Path
            updated_base_model_path: Path
            training_data: Path
            params_epochs: int
            params_batch_size: int
            params_is_augmentation: bool
            params_image_size: list

    5. Update the configuration manager in src/config

        Update ConfigurationManager Class and add get_training_config method which returns TrainingConfig type object.

    6. Update the components

        We will create a Training Class in a components file which will take TrainingConfig type object as argument and have the methods to perform the following tasks:

            Train Val Split
            Save the Model
            Train the Model etc.


    7. Update the pipeline

        Create a pipeline class for Training stage which integrates the whole process.

    8. Update the main.py

        In the main file, create an object of the class created in 7(Update pipeline) and call its main function. 

    9. Update the dvc.yaml

## Setting Up DVC

dvc (Data Version Control) is a git like framework for your code pipelines. It helps organize code in sucha a way that on re-execution, only the stages where any changes have been made are run saving time and computation power. To set-up dvc create a dvc.yaml file as shown and now when running the program, use 'dvc repro' command instead of 'python main'.


