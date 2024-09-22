# *RAG Pipeline* Template

<img src="assets/rag_template.png" alt="Image of the pipeline">

### Introduction:

This repository provides a Retrieval-Augmented Generation (RAG) pipeline template designed to handle user queries,
retrieve relevant information, and generate detailed responses using OpenAI models
in [Dataloop Platform](https://dataloop.ai/).
The pipeline integrates multiple components, including a Gradio app, an OpenAI text embedder, a retriever, and a ChatGPT
model.

## Components

### 1. Gradio App

The Gradio interface serves as the user-facing part of the pipeline. It allows users to communicate by inputting
free-text questions, to which the model will provide detailed responses. The app is designed for ease of use and
interaction.

#### To reach the gradio:

- Open the RAG Pipeline and click on `Start`.
- Select the `Gradio Start` node, then in the left panel, click `Open Gradio App`.

<img src="assets/open_gradio_pipeline.png" alt="set_up_parameters.png">

- A new window will appear. In the `Textbox`, enter your question. You will receive the answer in the same interface.


  <img src="assets/gradio__app.png" alt="set_up_parameters.png">

### 2. OpenAI Text Embeddings

OpenAI's `text-embedding-3` model is used to convert user questions into embeddings. These embeddings serve as the
foundation for retrieving relevant information from a dataset.

- **How it works**:
    - The user input is processed and embedded.
    - Embeddings are passed to the retriever for further analysis.

### 3. Retriever

The retriever is responsible for finding the most relevant items from a predefined dataset. It takes the following
inputs:

- **Embeddings**: Generated from the OpenAI model based on the user question.
- **Dataset ID**: The dataset from which the retriever will look for information.
- **Feature Set ID**: The specific feature set within the dataset.
- **Query Filter**: Optional filtering DQL to refine the search results.
- **K Nearest Items**: Defines how many of the closest items to return based on the similarity of embeddings.

#### To edit these parameters on the pipeline node:

1. Click on one of the input parameters of the `retriever` node to add the variables to the pipeline:

<img src="assets/set_up_parameters.png" alt="set_up_parameters.png">

2. Insert the values of the `dataset_id`, `featureset_id`, `k nearest items` and save changes.

<img src="assets/pipeline_variables.png" alt="pipeline_variables.png">

to get your `feature_set_id` use the SDK:

```python
import dtlpy as dl

project = dl.projects.get(project_name= < your - project - name >)
fs_id = project.feature_sets.get(feature_set_name= < your - fs - name >).id
```

3. Then, assign each pipeline variable for each input parameter on the node:

<img src="assets/choose_variable.png" alt="choose_variable.png">

#### A Preprocess stage is to create a feature set to the dataset using OpenAI's `text-embedding-3` embedder.

### 4. ChatGPT Model

Once the retriever finds the most relevant items, a ChatGPT model generates a comprehensive response. The model:

- Summarizes the retrieved information.
- Answers the user's question based on the system's predefined prompt and the retrieved results.

#### You can modify the model's system prompt in the Model Management tab under the Configuration section on the model's page:

<img src="assets/edit_system_prompt.png" alt="Image of the pipeline creation page">

An optional prompt:

```plaintext
"You are an AI assistant designed to answer questions using a provided set of documents. Your primary goal is to deliver precise and accurate responses, citing the relevant document sources for each answer. If the documents do not contain the necessary information, inform the user accordingly. Maintain a friendly and professional demeanor in all interactions."
```

### Installation:

In order to use the template, you need to follow these steps:

* Open the pipelines page and select Create Pipeline.
* Select Use a Template from the dropdown list.

<img src="assets/pipeline_create.png" alt="Image of the pipeline creation page">

* In the search bar, type `RAG`, select the template and click install.
* Once the template is installed, click on *Create Pipeline*.

