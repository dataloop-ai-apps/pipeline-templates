# *Image Metadata Enrichment* Pipeline Template

<img src="assets/pipeline.png" alt="Image of the pipeline">

### Introduction:

The Image Metadata Enrichment Pipeline Template automates the process of enriching image metadata with EXIF data and quality scores. The pipeline processes each image through two parallel branches:

- **EXIF Extractor** – Extracts embedded EXIF metadata from images (e.g., camera model, GPS coordinates, exposure settings, timestamps).
- **Quality Scores Generator** – Calculates image quality scores, including **Blurriness** and **Darkness** metrics.

The extracted information is added to the item's metadata, making it available for filtering, querying, and downstream processing within the Dataloop platform.

### Installation:

There are two options for installing and using the template:

1. Pipelines:

* Open the Pipelines page and select Create Pipeline.
* Select Use a Template from the dropdown list.

<img src="assets/pipeline_create.png" alt="Image of the pipeline creation page">

2. Marketplace:

* In the search bar, type `Image Metadata Enrichment`, select the template and click install.
* Once the template is installed, click on *Create Pipeline*.

<img src="assets/marketplace.png" alt="Image of the marketplace">
