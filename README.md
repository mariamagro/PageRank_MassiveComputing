# PageRank algorithm
In collaboration with [alexgaarciia](https://github.com/alexgaarciia)
## What is PageRank?
The PageRank algorithm is a technique used by web search engines to rank web pages in their search results. It was named after Larry Page, one of the founders of Google, and is used to estimate the importance of website pages. In order to understand this algorithm in depth, there are some definitions that must be grasped:

### How it works?
Generally, this algorithm works as follows:
1. Initial Assignment: Each page is given an initial rank. 
2. Link Evaluation: The algorithm evaluates all outbound links from a page. The PageRank value is equally distributed across these links.
3. Rank Calculation: The PageRank value for each page is calculated based on the PageRanks of pages linking to it. 

## Implementing PageRank
1. Exploration of the dataset.
2. Identification of links.
3. Create the Forward Links table.
4. Create the Backward Links table.
5. PageRank algorithm.

### 1. Exploration of the dataset
An initial exploration of the provided Wikipedia dataset can offer valuable insights into its structure and content.

The dataset includes columns "title," "id," "revisionId," "revisionTimestamp," "revisionUsername," "revisionUsernameId," and "text." The "title" column likely represents the title of the Wikipedia page, while "id" and "revisionId" are identifiers. The "revisionTimestamp" and "revisionUsername" provide information about the latest revision, and "text" contains the content of the Wikipedia article. The important information which will be used is stored in the columns named “title”, “id” and “text”.

### 2. Identification of links
To analyze the connectivity between Wikipedia pages and effectively implement the PageRank algorithm, we need to extract outgoing links from the textual content of each article. A **User-Defined Function (UDF)** will be crucial to achieve this, as it allows us to apply a customized analysis to the "text" field of our dataset. This analysis task involves identifying and extracting links enclosed within double brackets. This UDF will be a crucial step in the preparation for constructing the subsequent direct links matrix, essential for the implementation of the PageRank algorithm.

The "text" column holds the raw content of Wikipedia articles, including descriptions, citations, and links enclosed in double brackets ([[link]]). Extracting these links is crucial for analyzing the necessary relationships between articles, forming the basis for the PageRank.

### 3. Create the Forward Links table
It might be a good idea to provide some background information regarding what is a "Forward Links" table. This is typically used to represent the structure of links or connections from one entity to another. In our case, a forward links table is used to map each Wikipedia page to all the entities it directly links to. For a Wikipedia page, these entities would be other Wikipedia pages it references or links to.

The table structure tends to have:
1. ID Column: Contains unique identifiers for each page.
2. Links Columns: Contains a list of IDs to other pages that the current page links to; these are the "forward links".

This might be a little bit vague, but it can be clear with an example. Consider a Wikipedia page about "Artificial Intelligence" with ID "AI525". This page might have links to other pages like "Machine Learning", "Alan Turing", and "Neural Networks". The Forward Links table would then have an entry like: 
  - ID: AI525
  - Links: [ML456, AT789, NN101]

### 4. Create the Backward Links table
This part of the project revolves around reversing the Forward Table that has been just created in order to store the information about "backward" links: the links from other webs to a specific web. This will be useful so that both incoming and outgoing links are taken into account when considering the rank of the web.

### 5. PageRank algorithm
Now we are facing the last part of the project, where we must fully implement the **PageRank algorithm**. In order to keep it organized, we thought the best way of doing so would be to divide it into two parts: `First steps` and `Repeated application of the algorithm` (until convergence or max iterations are reached).

#### First steps
In this very first part, we will be setting up the initial environment or, in other words, the conditions for the PageRank algorithm to start iterating. This involves defining variables along the lines of: 
- `Damping Factor`: A constant used in the PageRank formula, representing the probability that a user continues to click on links. It is typically set to 0.85, as stated at the beginning of this project (please refer to this part for more information about the algorithm).
- `Max Iterations`: A threshold for the maximum number of iterations the algorithm should perform. It is a stopping criterion to prevent infinite looping in case the algorithm does not converge.
- `Threshold`: A small value that determines when to stop the iterations. If the change in PageRank values between iterations is less than this threshold, the algorithm is considered to have converged. 
- `PageRank Values`: Each page in the network is assigned an initial rank value, often set as the damping factor divided by the total number of pages.

#### Repeated application of the algorithm
Once the first part of this section is done, we can now start applying the algorithm over and over. To accomplish this, we may give some indications on what we are trying to perform below:
1. Computing the Count of Links: First off, we may want to count the outbound links for each page to **make this information efficiently accessible** during the PageRank computation. We use `ForwardDF`, which contains information about the forward links of each page. Then, a DataFrame with two columns (id, count) is created and converted into a Pandas DataFrame. Finally, we considered broadcasting the count data, since it avoids the need to send this data over the network to each worker node multiple times.

2. Defining the PageRank Function: This constitutes the most important part of this last section. This function calculates the new PageRank for a given page. It has 3 parameters:
   - `list_of_ids`: Contains the IDs of the pages that link to the current page.
   - `current_id`: ID of the page for which the new PageRank is being calculated.
   - `current_page_rank`: Pandas DataFrame containing the current PageRank of all pages.

3. Iterate until the maximum number of iterations is reached or convergence is achieved.
4. Display the new dataframe.

## Conclusions
The final insights can be seen in the final part of the project.
