# Square Root Descent

## Objective:
Find the square root without any tranditional python libraries

# Highlights and Embeddings
This directory is for generating Embeddings and Highlights on news corpus.

For DAG process, ETL team can use spark_news_process_for_dag.ipynb. This has an option of getting input and output parquet path from the set environment variable.
The path of notebook used for DAG process is 
"DraupMLmodels/news_processing/highlights_and_embeddings/spark_news_process_for_dag.ipynb"

#### Bootstrap file location: 
s3://draup-ml-datahouse/project_bootstrap_files/news_processing/news_highlights_embeddings_bootstrap.sh

For Highlights,
```
# SPARK #
# ===== #
# Adding Highlights notebook to SC for using the necesary functions
sc.addPyFile("s3://ml-pyspark-notebooks/e-BVAKKN17TARF22AZOKQ3NG62F/DraupMLmodels/news_processing/highlights_and_embeddings/news_highlights/generate_highlights.py")

# Importing the highlights function
from generate_highlights import get_highlights
```
```
# PYTHON #
# ====== #
import sys
sys.path.append("/home/notebook/work/DraupMLmodels/news_processing/highlights_and_embeddings/news_highlights/")

# Importing the highlights function
from generate_highlights import get_highlights
```
PS: No preprocessing or postprocessing is needed for using the shown function

*the input spark dataframe should contain the following,*
|News Title|News Description|...|
|----------|----------------|---|
|str      |str            |any columns|

*the output spark dataframe will additional columns like the following,*
|News Title|News Description|Highlights|...|
|----------|----------------|----------|---|
|str      |str            |list(str) |any present columns|


For Embeddings, 

```
# SPARK #
# ===== #
# Adding USE embeddings generator notebook to SC for using the necesary functions
sc.addPyFile("s3://ml-pyspark-notebooks/e-BVAKKN17TARF22AZOKQ3NG62F/DraupMLmodels/news_processing/highlights_and_embeddings/news_embeddings/generate_use_embeddings.py")

# Importing the embeddings function
from generate_use_embeddings import get_use_embed_from_str
```
```
# PYTHON #
# ====== #
import sys
sys.path.append("/home/notebook/work/DraupMLmodels/news_processing/highlights_and_embeddings/news_embeddings/")

# Importing the embeddings function
from generate_use_embeddings import get_use_embed_from_str
```
PS: No preprocessing or postprocessing is needed for using the shown function
*the input spark dataframe should contain the following,*
|News Title|Highlights|...|
|----------|----------|---|
|str      |list(str) |any columns|

*the output spark dataframe will additional columns like the following,*
|News Title|Highlights|Title Embedding|Highlights Embedding|...|
|----------|----------|---------------|--------------------|---|
|str      |list(str) |list(float)   |list(float)        |any present columns|



## Function overview
```
spark_transform_highlights(sp_news_df,
                           title_col= "title",
                           desc_col= "news_description",
                           out_col = "highlights",
                           num_sentences= 10, 
                           number_of_highlights= 5
                           )                  
     
    __Parameters__:
        sp_news_df: Input Spark DataFrame
        title_col: -str- Column name of the news title
        desc_col: -str- Column name of the news description
        out_col: -str- Output column of highlights
        num_sentences: -int- Number of sentences the summarizer should give at maximum
        number_of_highlights: -int- Number of highlights the algorithm should return at maximum

    __Mandatory requirements to run highlights function__:
        sp_news_df needs the following columns in the dataframe
            - News Title Column (Type: String)
            - News Description Column (Type: String)
    
    __Returns__:
        spark dataframe with an additional out_col with highlights
```
```
spark_transform_embeddings(highlights_res_df,
                           title_col = "title", 
                           highlights_col = "highlights",
                           title_embed_out_col = "title_embed",
                           highlights_embed_out_col = "highlights_embed"
                           )

    __Parameters__:
        highlights_res_df: Input Spark Dataframe with highlights generated
        title_col: -str- Column name of the news title
        highlights_col: -str- Column name of the generated highlights (list of strings)
        title_embed_out_col: -str- Output column name of the news title embedding (list of floats of 512 DIM)
        highlights_embed_out_col: -str- Output column name of the news description embedding (list of floats of 512 DIM)

    __Mandatory requirements to run embeddings function__:
        highlights_res_df needs the following columns in the dataframe
            - News Title Columns (Type: String)
            - News Highlights Columns (Type: List(String))
            
    __Returns__:
        spark dataframe with an additional title_embed_out_col, highlights_embed_out_col with title and highlights embeddings
```  
