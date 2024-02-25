# navigator

# Task Atlantbh by Elma Turčinović

## Summary

### Introduction

Our task involves analyzing a dataset to categorize each record based on quality (high, medium, low) without cleaning the data. We will use field validations, pattern recognition, and data standardization checks, along with the external verification methods. The goal is to negotiate lower prices for lesser-quality records while being willing to pay a premium for high-quality data. Also, this is the document where we will describe our validation methods, grouping criteria, and the distribution of data quality.

In the further section we will be discussing the results and insights gathered from the task we have been working on. Furthermore, we will give a detailed explanation of our thought process and decision making. 


###  Technology

Primary programming language used for this task was Python along with Jupyter Notebooks. Most noteable libraries from Python that we have used are:

- pandas (for data manipulation)
- matlplotlib.pyplot (for data visualization)
- jaro (Python library used primarily to handle string similarity using Jaro-Winkler distance)

Alternatives that could have been used were PySpark and plain SQL, but we didn't see a need to do so, because we were handling a very limited amount of data.

## Detailed explanation

### Criteria for data quality assessment
#### Low  quality data  criteria
 Checking for null values - This becomes very significant when we are talking about low quality data. We determined that a low quality data is missing at least one of the following :

- `Name`
- `ID`
- `address` and `geolocation` 

We have to mention couple of edge cases here as well. First one being, if by any chance a record doesn't contain `ID`, but contains `inspection_id`, then we can derive the ID of the company from the part of the `inspection_id`. If this is the case then the record passes the test and we consider it to be at least a `medium` quality data. 
The second case that we had to cover is the following: if a record has either the `address` or the `geolocation`, then we check if one can be derieved from the other. If thats the case, the record is set to be `medium` quality, and if not the quality is set to `low`. If the record has both the `address` and `geolocation` fields, we have to check if they are matching. 
For those purposes we have used  [Nominatim API](https://nominatim.openstreetmap.org/ui/search.html) to test this. Additionaly, for measuring similarity between address name from the csv and the address from the API we have used Jaro-Winkler distance.

#### Medium and high quality data criteria
If the conditions for the section above were not met, then we would consider that data to be low quality. Now we are handling only data that passed the initial low quality data check. In order to differentiate medium and high quality data, we had to make criteria for that. For those purposes we came up with several standards that we thought data had to meet in order to become high quality. 

One can imagine our process as the following. Let's imagine that we had ten criteria points and we have one of the cases  - if all of the ten criteria points are met then we mark that data as high quality. or if at least one of the criteria points are not met we mark that data as medium quality.

List of the criteria that we considered to be important:

- All of postal codes that have a state are either five or nine digits long (source: [IBM Documentation](https://www.ibm.com/docs/en/datacap/9.1.8?topic=data-example-us-postal-codes))
- All of the `violation_ids` have to have a `inspection_score`

## Results

As a final part of the task here are the percentages of the high, low and medium quality data:

- *High quality* : `41.959285% (21951)`
- *Medium quality* : `32.782185% (17150)`
- *Low quality* :  `25.258530% (13214)`

The analysis reveals a substantial portion of the dataset is of high quality, constituting nearly `42%` of the total, indicating a significant amount of the data meets our quality criteria. Medium quality data accounts for approximately `33%`, suggesting a need for minor improvements or adjustments. The remaining `25%` of the data is classified as low quality, highlighting areas where negotiation for lower prices or further data refinement might be necessary.
