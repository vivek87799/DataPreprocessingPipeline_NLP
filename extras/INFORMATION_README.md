# Translational Memory Exchange
- a database that stores sentences or paragraphs or segments of text
that have been translated before.
- each entry or segment includes its original language or "source"
and the translated language or "target"
- One of the data sources to be used in machine translation model
training process

# Task 1: Data Preprocessing Pipeline
Extraction
- Given a large TMX file, parallel translated segments should be extracted and prepared for the next step
- Use Etree lib with multiprocessing to extract the xml file and push it into RabbitMQ queue
- push the extracted data into RabbitMQ (simple and light compared to kafka) queue. Mark both queue and messages as durable
 
Cleaning
Writing
 
Scaling
- Should also run on local machine with limited RAM