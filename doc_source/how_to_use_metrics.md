# How to use Amazon File Cache metrics<a name="how_to_use_metrics"></a>

The metrics reported by Amazon File Cache provide information that you can analyze in different ways\. The list following shows some common uses for the metrics\. These are suggestions to get you started, not a comprehensive list\.


| How Do I Determine\.\.\. | Relevant Metrics | 
| --- | --- | 
| My cache's throughput? | SUM\(DataReadBytes \+ DataWriteBytes\)/Period \(in seconds\)  | 
| My cache's IOPS? | Total IOPS = SUM\(DataReadOperations \+ DataWriteOperations \+ MetadataOperations\)/Period \(in seconds\) | 