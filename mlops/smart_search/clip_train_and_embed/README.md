from secrets import token_urlsafe

# *CLIP Fine-tuning* Template

<img src="assets/pipeline.png" alt="Image of the pipeline">

### Introduction:

CLIP fine-tuning leverages the advantages of OpenAI's CLIP for your VLM needs, and fine-tunes the model on your custom
dataset. This template is sets up the fine-tuning pipeline with CLIP, and also provides the option to embed a dataset
with the fine-tuned model.

### Preparation

To be able to train CLIP, you need to have two datasets: one for images and one for the image and text pairs in 
prompt items. Please see the CLIP preprocessing pipeline for assistance in preparing these datasets.

### Installation:

To use the template, follow these steps:

* Open the Pipelines page and select _**Create Pipeline**_.
* Select _**Use a Template**_ from the dropdown list.

<img src="assets/pipeline_create.png" alt="Image of the pipeline creation page">

* In the search bar, type `CLIP Fine-tuning Template`, select the template and click _**Install**_.
* Once the template is installed, click on _**Use Template**_ at the top or _**Create Pipeline**_ at the bottom.
* Fill in the required IDs and press "Start Pipeline" to activate the pipeline.
* To run the pipeline, use the SDK with the following lines:

```python
import dtlpy as dl
project = dl.projects.get('<your_project_name>')
pipeline = project.pipelines.get(pipeline_name='CLIP Fine-tuning Template')
pipeline.execute()
```

<img src="assets/marketplace_create_pipeline.png" alt="Image of the pipeline in Marketplace">


## Attribution

This application uses OpenAI's CLIP, which is licensed under the MIT License. CLIP is a powerful open-source model for
image and text understanding developed by OpenAI.

### Acknowledgments
The CLIP model and code are the intellectual property of OpenAI.

Thank you to the contributors of the CLIP project for their work in advancing multimodal AI research.