# Solution Overview
Briefly describe what the project is about (e.g., running a Hadoop MapReduce job on Dataproc).
Lab 2 - Convert WordCount tutorial to URLCount

In this lab I was tasked with using MapReduce to count the number of URL in a document. This basically meant that all we had to really do was manipuate the map function within the UrlCount.java class to use regex to capture the links instead of words like the normal MapReduce methodology does. That is equated to the code in the map function. You basically just use regex to find patterns of (href="url_here") and only capture the url_here text and write out the key value pair (url_here, 1) which then gets combined before being passed to the reduce step. 

```java
public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        String line = value.toString();
        Pattern pattern = Pattern.compile("href=\"([^\"]+)\"");
        Matcher matcher = pattern.matcher(line);
        while(matcher.find()) {
            String extractedUrl = matcher.group(1);
            url.set(extractedUrl);
            context.write(url, one);
        }
        
    }
  }
```

To execute the UrlCount code. I created the data cluster, logged into the masters node, ran make, make prepare, and then time make run to run the code but also the time it took to run the UrlCount on the HTML files. 


## Software Requirements
List the software/tools needed to run your solution:
- Java 
- Hadoop 
- Linux command to run Makefile in GCP

## Resources Used
- Cluster name: test-dataproc
- Master node: test-dataproc-m
- Project ID: qwiklabs-gcp-01-ce893d56691a
- Region/zone : europe-west4
- Number of workers tested: 2 & 4

## Collaboration
I worked alone on this assignment along with Copilot.

## Sample Output
#### 2 & 4 workers both rendered the same output

| URL / Key | Count |
|---|---:|
| `https://en.wikipedia.org/wiki/Doi_(identifier)` | 18 |
| `https://en.wikipedia.org/wiki/Google_File_System` | 6 |
| `https://en.wikipedia.org/wiki/ISBN_(identifier)` | 18 |
| `https://en.wikipedia.org/wiki/MapReduce` | 6 |
| `https://en.wikipedia.org/wiki/S2CID_(identifier)` | 14 |
| `mw-data:TemplateStyles:r1295599781` | 33 |
| `mw-data:TemplateStyles:r1333133064` | 7 |
| `mw-data:TemplateStyles:r1333433106` | 121 |
| `mw-data:TemplateStyles:r886049734` | 12 |


## Execution Time Comparison
2 workers:               4 workers: 
real    1m6.563s         real    1m22.000s
user    0m21.358s        user    0m22.424s
sys     0m1.654s         sys     0m1.300s



## Discussion
The execution time for the 2 workers was faster compared to the execution of the 4 workers. This was very surprising to me because I would expect the 4 workers to be faster than the two because more nodes measns more parallel processing capacity. Possible reasons for this occuring would be that there are architectural bottlenecks where handling multiple machines outweights the benefit of having greater/faster computation. 

## Conclusion
In this lab I learned the power of MapReduce and how it can be applied to a wide array of problems of counting text in files that could provide significant value to the user. But my favorite part of the lab was struggling within the Google Cloud Platform because I learned a lot about how to handle and perform computation on big datasets within the GCP which is exactly what I want to get out of this class.
