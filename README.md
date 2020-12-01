# Square Root Descent

## Objective:
Find the square root without any tranditional python libraries



# Highlights and Embeddings
This directory is for generating Embeddings and Highlights on news corpus.

For general purpose, spark_news_process.py will have the necessary functions to transform the given News data into Highlights and Embeddings.

For DAG process, ETL team can use spark_news_process_for_dag.ipynb. This has the same utils found in spark_news_process.py but with an option of getting input and output parquet path from the set environment variable.


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
        title_embed_out_col: -str- Column name of the news title embedding (list of floats of 512 DIM)
        highlights_embed_out_col: -str- Column name of the news description embedding (list of floats of 512 DIM)
    __Mandatory requirements to run highlights function__:
        highlights_res_df needs the following columns in the dataframe
            - News Title Columns (Type: String)
            - News Highlights Columns (Type: List(String))
```
    
### Bootstrap file location
    s3://draup-ml-datahouse/project_bootstrap_files/news_processing/news_highlights_embeddings_bootstrap.sh
