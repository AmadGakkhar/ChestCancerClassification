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


## Model Evaluation

    1. Update config.yaml
    
        No configuration required for evaluation.

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

        Return type of config/param file information for Evaluation. Created using dataclass


        @dataclass(frozen=True)
        class EvaluationConfig:
            path_of_model: Path
            training_data: Path
            all_params: dict
            mlflow_uri: str
            params_image_size: list
            params_batch_size: int

    5. Update the configuration manager in src/config

        Update ConfigurationManager Class and add get_evaluation_config method which returns EvaluationConfig type object.

    6. Update the components

        We will create a Evaluation Class in a components file which will take TrainingConfig type object as argument and have the methods to perform the following tasks:

            Evaluate the Model on Evaluation Data
            Save Score in json format
            Log information to MLFlow.

    7. Update the pipeline

        Create a pipeline class for Evaluation stage which integrates the whole process.

    8. Update the main.py

        In the main file, create an object of the class created in 7(Update pipeline) and call its main function. 

    9. Update the dvc.yaml

## Set Up MLFlow

- [Documentation](https://mlflow.org/docs/latest/index.html)

- [MLflow tutorial](https://youtube.com/playlist?list=PLkz_y24mlSJZrqiZ4_cLUiP0CBN5wFmTb&si=zEp_C8zLHt1DzWKK)

##### cmd
- mlflow ui

### dagshub
[dagshub](https://dagshub.com/)

MLFLOW_TRACKING_URI= \
MLFLOW_TRACKING_USERNAME= \
MLFLOW_TRACKING_PASSWORD= \
python script.py

Run this to export as env variables:

```bash

export MLFLOW_TRACKING_URI=

export MLFLOW_TRACKING_USERNAME= 

export MLFLOW_TRACKING_PASSWORD=

### MLFlow on AWS Setup [OPTIONAL]

1. Login to console.
2. Create IAM User with Administrator access.
3. Export credentials in your AWS CLI by running **aws configure.**
4. Create s3 bucket.
5. Create EC2 Machine (Ubuntu) and add security groups (5000 port).
    Select instance -> Goto security -> Select security group -> Edit inbound rules -> Add rules -> Add port number 5000 and select 0.0.0.0/0 -> Save rules

Run the following commands on EC2 Machine

''' bash
sudo apt update
sudo apt install python3-pip
sudo pip3 install pipenv
sudo pip3 install virtualenv
mkdir mlflow
cd mlflow
pipenv install mlflow
pipenv install awscli
pipenv install boto3
pipenv shell
aws configure

mlflow server -h 0.0.0.0 --default-artifact-root s3://mlflow-bucket-2623

Open public IPv4 DNS to port 5000

Set uri in local terminal and in your code

export ML_FLOW_TRACKING_URI=http://ec2-3-142-248-17.us-east-2.compute.amazonaws.com:5000

update remote_server_uri in your code as well to see the tracking on aws server.

## Setting Up DVC

dvc (Data Version Control) is a git like framework for your code pipelines. It helps organize code in sucha a way that on re-execution, only the stages where any changes have been made are run saving time and computation power. To set-up dvc create a dvc.yaml file as shown and now when running the program, use 'dvc repro' command instead of 'python main'.

## AWS-CICD-Deployment-with-Github-Actions

### 1. Login to AWS console.

### 2. Create IAM user for deployment

	#with specific access

	1. EC2 access : It is virtual machine

	2. ECR: Elastic Container registry to save your docker image in aws


	#Description: About the deployment

	1. Build docker image of the source code

	2. Push your docker image to ECR

	3. Launch Your EC2 

	4. Pull Your image from ECR in EC2

	5. Lauch your docker image in EC2

	#Policy:

	1. AmazonEC2ContainerRegistryFullAccess

	2. AmazonEC2FullAccess

	
### 3. Create ECR repo to store/save docker image
    - Save the URI: 566373416292.dkr.ecr.us-east-1.amazonaws.com/chicken

	
### 4. Create EC2 machine (Ubuntu) 

### 5. Open EC2 and Install docker in EC2 Machine:
	
	
	#optinal

	sudo apt-get update -y

	sudo apt-get upgrade
	
	#required

	curl -fsSL https://get.docker.com -o get-docker.sh

	sudo sh get-docker.sh

	sudo usermod -aG docker ubuntu

	newgrp docker
	
### 6. Configure EC2 as self-hosted runner:
    setting>actions>runner>new self hosted runner> choose os> then run command one by one


### 7. Setup github secrets:

    AWS_ACCESS_KEY_ID=

    AWS_SECRET_ACCESS_KEY=

    AWS_REGION = us-east-1

    AWS_ECR_LOGIN_URI = 

    ECR_REPOSITORY_NAME = {second part of URI}


