# Tutorial - Deploy Mistral-Small-3.1-24B-Instruct using Inferless
​Mistral-Small-3.1-24B-Instruct is an advanced multimodal language model developed by Mistral AI, featuring 24 billion parameters. Building upon its predecessor, Mistral Small 3, this model introduces enhanced text performance, robust vision capabilities, and an expanded context window of up to 128,000 tokens. It excels in various tasks, including image analysis, programming, mathematical reasoning, and supports over two dozen languages. 

## TL;DR:
- Deployment of  model using [transformers](https://github.com/huggingface/transformers/).
- Dependencies defined in `inferless-runtime-config.yaml`.
- GitHub/GitLab template creation with `app.py`, `inferless-runtime-config.yaml` and `inferless.yaml`.
- Model class in `app.py` with `initialize`, `infer`, and `finalize` functions.
- Custom runtime creation with necessary system and Python packages.
- Recommended GPU: NVIDIA A100 for optimal performance.
- Custom runtime selection in advanced configuration.
- Final review and deployment on the Inferless platform.

### Fork the Repository
Get started by forking the repository. You can do this by clicking on the fork button in the top right corner of the repository page.

This will create a copy of the repository in your own GitHub account, allowing you to make changes and customize it according to your needs.

### Create a Custom Runtime in Inferless
To access the custom runtime window in Inferless, simply navigate to the sidebar and click on the Create new Runtime button. A pop-up will appear.

Next, provide a suitable name for your custom runtime and proceed by uploading the **inferless-runtime-config.yaml** file given above. Finally, ensure you save your changes by clicking on the save button.

### Import the Model in Inferless
Log in to your inferless account, select the workspace you want the model to be imported into and click the `Add a custom model` button.

- Select `Github` as the method of upload from the Provider list and then select your Github Repository and the branch.
- Choose the type of machine, and specify the minimum and maximum number of replicas for deploying your model.
- Configure Custom Runtime ( If you have pip or apt packages), choose Volume, Secrets and set Environment variables like Inference Timeout / Container Concurrency / Scale Down Timeout
- Once you click “Continue,” click Deploy to start the model import process.

Enter all the required details to Import your model. Refer [this link](https://docs.inferless.com/integrations/git-custom-code/git--custom-code) for more information on model import.

---
## Curl Command
Following is an example of the curl command you can use to make inference. You can find the exact curl command in the Model's API page in Inferless.
```bash
curl --location '<your_inference_url>' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer <your_api_key>' \
    --data '{
    "inputs": [
        {
            "name": "prompt",
            "shape": [
                1
            ],
            "data": [
                "What is an AI?"
            ],
            "datatype": "BYTES"
        },
        {
            "name": "temperature",
            "optional": true,
            "shape": [
                1
            ],
            "data": [
                0.7
            ],
            "datatype": "FP64"
        },
        {
            "name": "top_p",
            "optional": true,
            "shape": [
                1
            ],
            "data": [
                0.1
            ],
            "datatype": "FP64"
        },
        {
            "name": "repetition_penalty",
            "optional": true,
            "shape": [
                1
            ],
            "data": [
                1.18
            ],
            "datatype": "FP64"
        },
        {
            "name": "max_tokens",
            "optional": true,
            "shape": [
                1
            ],
            "data": [
                256
            ],
            "datatype": "INT64"
        },
        {
            "name": "top_k",
            "optional": true,
            "shape": [
                1
            ],
            "data": [
                40
            ],
            "datatype": "INT64"
        }
    ]
}'
```

---
## Customizing the Code
Open the `app.py` file. This contains the main code for inference. The `InferlessPythonModel` has three main functions, initialize, infer and finalize.

**Initialize** -  This function is executed during the cold start and is used to initialize the model. If you have any custom configurations or settings that need to be applied during the initialization, make sure to add them in this function.

**Infer** - This function is where the inference happens. The infer function leverages both RequestObjects and ResponseObjects to handle inputs and outputs in a structured and maintainable way.
- RequestObjects: Defines the input schema, validating and parsing the input data.
- ResponseObjects: Encapsulates the output data, ensuring consistent and structured API responses.

**Finalize** - This function is used to perform any cleanup activity for example you can unload the model from the gpu by setting to `None`.

For more information refer to the [Inferless docs](https://docs.inferless.com/).
