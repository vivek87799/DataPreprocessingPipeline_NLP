# Task 1: Data Preprocessing Pipeline
Extraction
- Given a large TMX file, parallel translated segments should be
extracted and prepared for the next step
 
Cleaning
Writing
 
Scaling
- Should also run on local machine with limited RAM
 
# Task2: Translation Web Service
Technical requirements
Translation server
- An endpoint "/translate_batch" that takes a json payload of source
sentence and return a json payload with target sentence
Translation client
- Accepts a list of source sentences and calls the server
asynchronously and returns the list of translate sentences in the
same order
 
 
 
 
 
 
Technical description
# Task 1
   Extracting
Etree with python multiprocessing to read the data from xml
Each process pushes the read data into RabbitMQ queue
   Cleaning
   -	
 
channel.queue_declare(queue='hello', durable=True)
 
Fair Dispatch
