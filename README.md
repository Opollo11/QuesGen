# QuesGen
## How we built it
Using Deep Learning Techniques of Natural Language Understanding and processing. For now we will be building a web app which can generate the questions based on the input. Our tech stack primarily will be python for backend and HTML, CSS, JS for our frontend. We also aim to incorporate symbl.ai Realtime transcript service into our application so as to get a better text from a noisy data. We also aim to use Twilio services to validate a user entering our system to prevent DDOS.

## Challenges we ran into
First, we have to search for an alternative for Google Speech-to-text api as it is not free beyond a certain limit. We also have not metric to check the accuracy of our model and have to rely completely on self-test. Integrating the models together and generating the final product. Other than that generating distractors or wrong choices is a task to be achieved.

## Accomplishments that we're proud of
We will make it capable of generating 4 different types of questions namely - MCQ, Boolean, Paraphrase and FAQ type. We also will try to extract text from various forms of input like PDF, Youtube links and realtime voice

## What we learned
We are working on the frontend and also working on the duplicates of the sentences and preventing from entering the database which can waste our computation power. Other than that we are trying to add a feature of OCR text recognition which will be useful when suppose a person has completed reading a book or the slides of the teacher they can self assess themselves with appropriate questions. For True or False Questions

1) Suppose we have a complex sentence we need to split that compound sentence to a simple sentence in order to get a better question. Example - He wiped off the water and turned the boat upside down. The simplified sentence would be He turned the boat upside down.

Q) How to falsify a particular sentence?

Ans) Example the amount of matter in a chemical reaction does not change. The falsified sentence would be the amount of matter in a chemical reaction changes. It can be done in two ways- 1) Add or remove negation of a verb phrase or noun phrase like the verb/action 2) Change a named entity like the year or name of the person 3) Change the adjective 4) Change the main verb 5) Alternate verb phrase or noun phrase

Examples

1) The amount of matter in a chemical reaction does not change. The falsified sentence would be the amount of matter in a chemical reaction changes.

2) when electrons are shared between two atoms a covalent bond is formed to when electrons arae transferred between two atoms, a covalent bond is formed.

3) Aunt avanti was sitting in an armchair waatching the rain washed tree and blue sky to Aunt avanti was sitting on the sofa watching television.

Section Generating Falsified sentence with alternate verb phrase or noun phrase

We can use the openai gpt-2 for sentence completion and will take initial sentence and generate this false alternate sentence. Constituency Parsing is the task of breaking a sentence to its constituents and sub-phrases. It helps us to break down the sentence on the noun phrase. Non terminals in the parse tree are types of phrases the terminals are the words in the sentence. The sentence is split into noun and verb phrases and the terminals are the words and the constituency are the words in the middle. We split the sentence in the end of the sentence and then generate the constituency parse tree is formed.

Q) What is Open AI GPT-2 ?

Ans) It is a text generation model and is a transformer based language model trained to predict the next word. The most famous use cases of transformer models are language translation, text generation, summarization, etc. The algorithm GPT-2 will predict any sentence and will generate a false question.

Q) How is bert being used for this project?

Ans) The problem is that we generate 10 sentence from open ai gpt-2. we need to use the most dissimilar sentence to be used as the false sentence.So we need to sort the sentences by order of similarity and then use the top 10 dissimilar sentence are used. So sentence embedding algorithms can be used where the sentence is converted into vector and compare it to the original question vector. We can use sbert.net / the bert model for this application. Sentence transformer model is used here we get the possible false sentence and use the scipy spatial distance function with cosine function to print the distance from the original sentence. we are then sorting the results. All this is done by BERT.

Generating wrong choices/distractors for a MCQ type of questions. The Distractors in our project is implemented through three methods 1) WordNet 2) Concept Net 3) Word Vectors

The main features of a distractor is that it should be homogeneous and mutually exclusive and thus no synonyms should be included.

Section WORDNET

Q) What is Wordnet?

Ans) Wordnet is a large lexical database of english and is similar to thesaurus but captures even broader relationships between words. Wordnet is also free and publicly available for download. It also captures the sense of the word like mouse can be both animal and a computer device so based on the sentence it captures the semantic relations among the words.

Q)How we can use Wordnet to generate this distractor?

Ans) Wordnet captures the relations. A hyponym is a type of relationship with its hypernym. A hypernym is in a type of relationship with its hypernym. It is a umbrella term or blanked term. Example Color is the hypernym for purple or red. Our goal is to extract co-hypernyms as distractors. We need to have the options under the same blanket.

Read more about wordnet here - https://wordnet.princeton.edu/

Working for the Related Notebook ie generating wrong options using wordnet

Link --

We are taking a single word which can have multiple usages like bat or nile. 1) So using synset we can actually find all kinds of relation the word has and various sentences which can be formed with that word.

2) Another usage of synsets is we can extract only like noun sense of that word in a given sentence

3) Using this synset we extract all the hypernyms of that word to be used as a distractor for the given question

Section CONCEPTNET

Q) What is ConceptNet?

Ans) Conceptnet is a free multilingual knowledge graph. It includes knowledge from crowdsourced resources and expert created resources. Conceptnet is also used to create word embeddings just like glove, word2vec, etc. Similar to wordnet, conceptnet labels the semantic relationships among words. They are also more detailed than wordnet ie is a part of , similar to, is used for, etc relations among words is captured.

Q) How can we use conceptnet to generate distractors?

Ans) Like the co- hyponymns in wordnet, we will be using the same semantic relationship feature of conceptnet. For example Texas, Arizona, New Mexico, Nevada, Oregon, Utah, Florida, Colorado etc are a part of United states of america. we find such similar words.

Read more about Conceptnet here - https://conceptnet.io/

Working for the Related Notebook ie generating wrong options using conceptnet

1) Using the API of conceptnet we are using this 2) Conceptnet is multilingual so we are trying to find the hypernym 3) Narrow down the search and extract only those parts of the words which have a part of relationship with the given word 4) We narrow down to the words which has part of relation with the given word 5) We print all part of relationships 6) we can then find the co hyponyms using conceptnet

Section WORD VECTORS

Q) what is word vectors?

Ans) Word vectors are words converted into a vector or array representation and these vectors capture associations among different kinds of words. Unlike wordnet and conceptnet these are not human curated and automatically generated from a text corpus. A neural network algorithm is trained with predicting the focus word given other words or predicting surrounding words given a focus word. The limitations are that all senses of a given word have only one vector. Word vectors are context independent.

Q) How is word vector is used to generate distractors?

Ans) Like what king is to a man, same is queen is to a woman. Like cricket is insect or game, we need to take only one representation of a given word. They are unable to isolate the senses for a given word. They cannot capture the sense of a given word.

Q) How to generate distractors using sense2vec?

Ans) Contextual information is captured. It is trained on reddit comments. The words with the same senses are differentiated with parts of speech eg duck (verb) and duck(noun). The noun phrases and named entities are annotated during training so multiword phrases like natural language processing also have an entry as opposed to some word vector algorihms which are trained with only single words. We are using a dataset of 2015 reddit comments for our application.

Working for the Related Notebook ie generating wrong options using sense2vec

1) We download the sense2vec 2) We get the word and then try to get the most popular sense for that word. 3) Distractors are then found usingn sense2vec

Section FILTERATION

We have to compute similarity between words using levenshtein distance.

1) We take out all kinds of words, take the root word, 2) calculate the leveshtein distance for each word and then check it properly

## What's next for EduHelper
We are working on the frontend and also working on the duplicates of the sentences and preventing from entering the database which can waste our computation power. Other than that we are trying to add a feature of OCR text recognition which will be useful when suppose a person has completed reading a book or the slides of the teacher they can self assess themselves with appropriate questions.

## References
We have used references from this github for forming our project - https://github.com/ramsrigouthamg/Questgen.ai
