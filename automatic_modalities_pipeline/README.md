# Automatic Modalities Pipeline Template

The Automatic Modalities Pipeline is to develop a highly efficient, scalable, and intelligent system capable of automatically processing and analyzing diverse data modalities (e.g., text, images, videos, and sensor data) to extract meaningful insights, support decision-making processes, and enhance predictive analytics capabilities. This pipeline will integrate advanced algorithms, including machine learning and artificial intelligence, to facilitate the seamless conversion of raw data into actionable information.

For every added item, a modality will be created automatically. To learn more, see the [Modality](https://dataloop.ai/docs/modality) article.

## Installation & Configuration

The installation of the Automatic Modalities Pipeline typically involve several key steps, such as preparation, execution, etc. to ensure that the system is correctly set up and optimized for processing and analyzing multi-modal data.


### Preparation

#### Step 1: Name Your Master Items

Use a filename system to connect master items to their references.

1. Name your master items with unique names.

**Note**: File names should not be included/part of other filenames. 
**For example**, if one master item is named `sensor.jpg`, you should not have another master file named `sensot2.jpg`.


2. Name the reference items to include the master items’ name. 
**For example**, the master item `sensor.jpg` will be referenced with `sensor_daytime.jpg` and `sensor_night_vision.jpg`. 


#### Step 2: Create a Folder in Your Dataset

Create a folder in your dataset that will contain all reference items.


#### Step 3: Create a Pipeline

Create a new pipeline using the template “auto-modality pipeline”. 


#### Step 4: Add an Event Trigger

Add an event trigger to the pipeline.

1. Click on the first node, which is marked by the play button. 
2. On the trigger section, click on the “+” button.
3. Select the Type -> **Event**, Action -> **Created**, and paste the following DQL.

```json
{
	"$and": [
		{
			"hidden":false
		},
		{
			"dir":"/master"
		},
		{
			"type":"file"
		}
	]
}
```

<img src="../assets/amp_add_event_trigger.png" alt="Image of the add an event trigger">


#### Step 5: Start the Pipeline

Click “Start Pipeline” and make sure it is running.


#### Step 6:  Create Another Folder 

Create a new folder on the same reference dataset called: `“/master”`.


#### Glossary

* A **master item** is an item that will receive overlaid items.
* **References** are the overlaid items.


### Execution

Now, every image you upload to the `“/master”` folder will automatically contain its modality a few seconds after.


## Nodes in this Pipeline

### Pre Modality 

Matches the master item to its references. 

* **Input**: item
* **Output**: 'pre_modality_result' object:

```json
{
	"master_item_id": <item id>,
	"ref_items_id_list": <reference items id list>
}
```

Item created on master directory trigger with the following DQL that added:


```json
{ // every created item on the "/master" directory will trigger the pipeline
	"$and": [
		{
			"hidden":false
		},
		{
			"dir":"/master"
		},
		{
			"type":"file"
		}
	]
}
```

### Modality Creation

An SDK script that creates the modality.

* **Input**: pre_modality_result object (as described above)
* **Output**: item

## Pipeline Adjustments

Update the input directory (master) or limit the trigger to work on a specific dataset. Update the trigger DQL on the Pre Modality node.


```json
{
	"$and": [
		{
			"hidden":false
		},
		{
			"dir":"/<dir name>"
		},
		{
			"type":"file"
		},
                {
			"datasetId": "<Dataset ID>"
		}
	]
}
```

Update the matching mechanism between the master and the references.

```py
def pre_modality(self, item, progress=None):
        filename, ext = os.path.splitext(item.name)
        # Filter value for matching the master item to the references items
        filters = dl.Filters(field="name", values="*{}*".format(filename))
        ref_items_id_list = list()
        pages = item.dataset.items.list(filters=filters)
        for ref_item in pages.all():
            if ref_item.id != item.id:
                ref_items_id_list.append(ref_item.id)
        pre_modality_results = {"master_item_id": item.id, "ref_items_id_list": ref_items_id_list}
        print("*** pre_modality: return pre_modality_results {}".format(pre_modality_results))
        return pre_modality_results 
```


## Contributions, Bugs and Issues - How to Contribute

We welcome anyone to help us improve this app.  
[Here's](CONTRIBUTING.md) a detailed instructions to help you open a bug or ask for a feature request.