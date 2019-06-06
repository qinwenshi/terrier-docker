#Terrier Jig

This Docker setups Terrier 5.2.



## Basic Runs

### Indexing:

	python run.py prepare     --repo terrier   --collections robust04=/tmp/disk45/disk5/LATIMES/=trectext

### Retrieval:

(BM25)

	python run.py search  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt   --output /tmp/runs 

(BM25 + query expansion)

	python run.py search  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt   --output /tmp/runs --opts config=bm25_qe

(PL2)

	python run.py search  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt   --output /tmp/runs --opts config=pl2

(PL2 + query expansion)

	python run.py search  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt   --output /tmp/runs --opts config=pl2_qe

## Learning to Rank Runs

Learning-to-rank will typically require that the index has more information, e.g. fields or blocks.

### Indexing:

	python run.py prepare     --repo terrier   --collections robust04=/tmp/disk45/disk5/LATIMES/=trectext --opts "FieldTags.process=HEADLINE"

### Training:

You need to specify the features to be used by Terrier - see http://terrier.org/docs/v5.1/learning.html for more information about Terrier feature definitions.

	python run.py train  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt    --test_split $PWD/sample_training_validation_query_ids/robust04_test.txt  --validation_split $PWD/sample_training_validation_query_ids/robust04_validation.txt --model_folder /tmp/runs --opts features="SAMPLE;WMODEL:SingleFieldModel(BM25,0);QI:SingleFieldModel(Dl,0)"

### Retrieval:

You will need to specify the `bm25_ltr_jforest` configuration.

	python run.py search  --repo terrier --collection robust04  --topic topics/topics.robust04.txt --qrels qrels/qrels.robust04.txt   --output /tmp/runs --opts config=bm25_ltr_jforest
