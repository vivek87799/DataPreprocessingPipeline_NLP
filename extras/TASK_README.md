# Coding challenge

## Introduction
Welcome to the challenge, dear future data engineer. In this task you will get a glimpse into the typical challenges you would be facing working day to day within the Machine Translation team.

As a data engineer at lengoo you will be creating and maintaning different data services on the python stack supporting the machine translation automated model training pipeline. These can be different customer requirements, pre-processing data, creating and improving the performance data microservices and defining deployment processes.

## Data preprocessing pipeline
The TMX (translation memory exchange) file format, a type of XML, is typical to the translation industry and it contains previously translated sentences. It is one of the data sources that can be used in the machine translation model training process. A sample TMX file is provided in this repo in `resources/tmx-file.tmx`

In the first task you will be asked to create a small pipeline for *extracting*, *cleaning* and *writing* the translated segments stored in the TMX file. This should be completed in a scalable manner, considering big data.

### Extracting
Given a TMX file, parallel translated segments should be extracted and be prepared for the next step.

### Cleaning
Typically real historic translation sentences contain noise and artefacts from e.g. a content management system, so it is important to clean and extract the meaningful and contentful data before feeding it into the machine translation model.

* Examples of non-clean segments:
	* XML elements mid-segment
		* `<seg>This segment <x/> is <g>great</g></seg>`
	* escaped HTML tags mid-segment
		* e.g. `<seg> This segment is &lt;br/&gt; great</seg>`
	* other markup within text:
		* e.g. `<seg>This segment is %link_start%great%link_end%. </seg>`
	* Correct extraction in all cases = "This segment is great."
	
Further noise may be discovered by empirical observation of the data.

As this noise is always very customer-specific and can contain various, unpredictable elements, your code base should be easily extendible with further rules by using some neat code structures and interface.

### Writing
The queued segments are then cleaned and utlimately written to any prefered parallel data structure/dump.

### Scaling up
The same TMX preprocessing pipeline should be also runnable locally (RAM-limited) on a bigger data set.

Your solution will be evaluated on the ParaCrawl https://object.pouta.csc.fi/OPUS-ParaCrawl/v1/tmx/de-en.tmx.gz TMX files which contains 36.4M sentences. The priority for this step is performance and runnability and not the cleaning rules.

* Functionality requirements:
	* Command line interface (CLI)
	* Takes a TMX file and produces parallel file structure
 	* 	* Example command:
		* `$ python extract.py --tmx-file file.tmx --output “./myfolder/output”` 
	* Expected output:
		* file object produced in `/myfolder`

## Translation web service
In a typical application, a web translation service is used, which has an endpoint that serves translations of cleaned source sentences. You will develop a simple translation web service with an endpoint that serves translations taken from the extracted cleaned parallel data dump. You will also develop a corresponding translation client.

### Technical requirements
Since translations can take some time to be retrived from the web service, the main challenge here is to use an asynchronous model. 

### Translation server
The translation server starts and runs an asynchronous REST server. It should be able to perform batch translations and as it works asynchronously, it should handle 100+ of requests per second. 

The endpoint `/translate_batch` should accept a request with a JSON payload of source sentences:

* Example:
	* `[{"src_sent": "My sentence 1."}, {"src_sent":"My sentence 2."}, {"src_sent":"My sentence 3."}]` 

This batch of source sentences should be translated and the server should return the following (normally, we would have a translation model running behind this, but for now, please use the extracted translation file(s) to simply look up the target sentence given the source sentence, in order to generate this output):

* Example:
	* `[{"src_sent": "My sentence 1.", "tgt_sent": "Mein Satz 1."}, {"src_sent": "My sentence 2.", "tgt_sent": "Mein Satz 2."}, {"src_sent": "My sentence 3.", "tgt_sent": "Mein Satz 3."}]` 

### Translation client
The translation client implements the functionality that accepts a list of source sentences, calls the server asynchronously and returns a list of translated sentences in the same order.

Furthermore, the client file has a main function that sends 100+ requests asynchroniously, measures and prints the time it took.

## Resource and materials
Below is a list of resources and materials that you might find useful in this challenge; however, please consider this list only as guidance and do not feel constrained to using necessarily these tools. We are interested in whatyou can come up with for this challenge.

* Extracting data from TMX files, `lxml`: https://lxml.de/tutorial.html
* For queueing data into the pipeline, `RabbitMQ`: https://www.rabbitmq.com/tutorials/tutorial-three-python.html
* For asynchronous data processing, `asynchio`: https://docs.python.org/3/library/asyncio.html
* For asynchronous data processing over http, `aiohttp`: https://aiohttp.readthedocs.io

## Challange evaluation

1. Extensible, using standardizeable formats
2. Clean, organized, and adhering to PEP8
3. Scalable to large data
4. With a clean API
5. With a clear readme and helpful documentation

