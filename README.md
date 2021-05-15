# IR-Algorithms
Implementation of some common scoring schemes and search algorithms in the field of Information Retrieval

## Folder Struture
```
.
├── Appendix
│   └── stories.zip
├── Boolean-Search
│   ├── Dumps
│   │   ├── docID.pkl
│   │   ├── docs.pkl
│   │   └── index.pkl
│   └── Notebooks
│       ├── Indexing.ipynb
│       ├── Preprocessing.ipynb
│       ├── Query.ipynb
│       └── substitutions.py
├── Phrase-Search
│   ├── Dumps
│   │   ├── positional_index.pkl
│   │   ├── stories_data.pkl
│   │   └── stories_docs.pkl
│   └── Notebooks
│       └── Positional_Index.ipynb
├── README.md
└── Vector-Search
    ├── Dumps
    │   ├── stories_data.pkl
    │   ├── stories_docs.pkl
    │   ├── tf_idf_index.pkl
    │   └── tf_idf_vocabulary.pkl
    └── Notebooks
        └── Scoring.ipynb
```

## Boolean Search :mag_right:

### Dataset Preprocessing

The index.html files are removed and 15 files from the SRE folder are shifted to stories folder. The resulting 467 files are preprocessed before creating the Unigram Inverted Index. NLTK library was used for preprocessing. Following steps are undertaken to clean the document text:-

- Text is converted to lower case and tokenized.
- Words with apostrophe are substituted with their appropriate phrases. (e.g. didn't : did not)
- Punctuation and unnecessary characters are removed.
- Stopwords are removed.
- Tokens are lemmatized.

The documents' names, original texts and cleaned texts obtained after preprocessing are stored in a pickle file `docs.pkl`.

### Index Creation

The preprocessed documents are now parsed for creating Unigram Inverted Index. The documents are in alphabetical order of their names and are assigned a numerical ID starting from 0. The mapping of DocID to Doc_Name is stored in `docID.pkl`. Posting lists are created for the terms and the terms are sorted in alphabatical order. The index created is stored in `index.pkl`.

### Query Processing

Separate functions have been created for the 5 types of operations: `AND`, `OR`, `NOT`, `AND NOT`, `OR NOT`. The index and docIDs are loaded from their respective dumps. The Query is processed from left to right.

**Input Format**  

```
Input 1 : Query Sentence (e.g. : lion stood thoughtfully for a moment)
Input 2 : Operators separted using ", " without any enclosing square brackets (e.g. : OR, OR, OR)
```

**Output Format**  

```
Number of documents matched: int
Number of comparisons required: int
Documents: [List of Document Names]
```

## Phrase Search :mag_right:

### Preprocessing
The index.html files are removed and 15 files from the SRE folder are shifted to stories folder. The resulting 467 files are preprocessed before creating the Positional Index. Following steps are undertaken to clean the document text:-

- Convert the text to lower case
- Perform word tokenization
- Remove punctuation marks from tokens
- Remove stopwords from tokens
- Remove blank space tokens

The documents' names, original texts and cleaned texts obtained after preprocessing are stored in a pickle file **stories_data.pkl**.

### Creating Positional Index
For creating the index we iterate through the cleaned document texts and store the document name as well as the position of the word in the document in the posting list of the term. 

### Query System
For query system, we first load the index from the saved pickle file. Then we create pointers for all the words in the query and try to find the documents which contain all the words of the query in the correct order using the positional index created.

## Vector Based Scoring & Search :mag_right:

### Setup
We load the cleaned stories data from the pickle file and first create an index for TF-IDf which stores document frequency of each term.
We then proceed to build TF-IDF matrices for the 5 different TF weighting schemes:-

<img width="475" alt="Screenshot 2021-04-16 at 22 18 33" src="https://user-images.githubusercontent.com/42066451/115057437-afe90600-9f01-11eb-8324-4dbc399bfa35.png">

The matrices are converted to numpy arrays and stored in pickle files.

### Query System

`TF-IDF` : The Query is cleaned and a query vector of size of Vocabulary is generated. Dot product is taken between the respective TF-IDF weight matrix and this vector to get scores of all documents. Top 5 documents are reported.

`Cosine Similarity` : The Query is cleaned and a query vector of size of Vocabulary is generated. Dot product is taken between the respective TF-IDF weight matrix and this vector to get scores of all documents. The score is cosine normalised by divding it with magnitudes of document and query vectors. Top 5 documents are reported.

`Jaccard Coefficient` : The document text and query text are represented as sets and scores are calculated as per formula for Jaccard Coefficient.
