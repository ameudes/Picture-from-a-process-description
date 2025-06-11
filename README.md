# Picture-from-a-process-description

![image](https://github.com/user-attachments/assets/fa1f63a1-d44c-441f-8726-8b25aa74476d)

## Description
This project consists in creating a system that can understand and transform a textual description of a process into an image.  A process is a sequence of steps or actions to achieve something. For example, the process for making a cake might be:  "mix the ingredients, then pour the batter into a tin, then put in the oven, wait for it to bake, and finally take the cake out of the oven". The aim is to create a tool capable of handling descriptions of procedures and producing a clear and precise visual representation of the different steps. The text input for this project are in french. 

## The approach used 
As the project is part of  a "Natural Language Processing" learning journey, it is approached from the angle of natural language understanding and text generation. The first step is to understand the description of the process in natural language, i.e. extract the relevant information and organize it in such a way that it can be represented as a graph or image. The next step is to automatically generate an image from this representation.

## Method
The project was divided into two steps: **the first step** consists in using NLP tools to process the corpus of the process, in order to extract useful and relevant information for the graphical representation of the process. The output of this step is a dictionary whose keys are all the steps in the process, and whose values for each key are all the steps preceding the step that serve as key. **The second step** takes as input the output of the first stage and provides the schema corresponding to the process described.

**Step 1: Processing the textual description of the process**
The algorithm used is inspired by available works in the literature. Schematically, the implemented algorithm can be represent as follow.

Thus, to obtain the expected output at this first step, the following path must be followed:
- Tokenization : the aim  is to transform our process description corpus into "tokens", which are assimilated here to the words in the corpus. This will allow to better understand the structure of the text and analyze it more easily.
- POS (Part Of Speech) tagging : once the process corpus has been broken down into words, POS tagging took place. This natural language processing technique allows to assign to each word in the description text a grammatical label according to its nature, helping to better understand the structure and meaning of each sentence in the corpus. This makes it easier to identify the actions and objects mentioned in the process description, and thus helps to create an accurate graphical representation of it. For example, if a word is identified as a verb, this may indicate an action to be represented, while a common noun may represent an object to be drawn.
- Identifying actions and connectors : at the end of the POS tagging, the grammatical analysis of each word in each sentence is carried out, enabling  to identify the grammatical class of each word, and to situate it in its context. Thus, assuming that each sentence in the corpus contains an action verb and a noun, one can clearly identify for each sentence, the action related to it in the process. An action here is defined as a "verb, noun" sequence. Once the actions have been listed, we need to introduce a logical, even chronological sequence. Hence the need to identify connectors in the corpus in order to classify the actions.
- Syntactic analysis and action classification : to correctly classify the actions listed in the previous section, we first need to perform a syntactic analysis to identify synonyms for certain tokens in the corpus. The aim here is to have a reduced vocabulary that takes into account the essentials of French vocabulary. By using the dictionary, we can define a single vocabulary in terms of logical connectors, to classify actions once the description text has been supplied and the previous steps applied.
- Action dictionary : once the actions have been classified, we can finally obtain the output of this first step by creating a key-value dictionary, following the principle described in step1.

**Note:** Due to certain constraints, the code associated with this project only implements tokenization, POS tagging, action identification and classification. The other stages could not be fully implemented. undefinedinputs. As a result, the proposed code contains certain limitations, and its ability to process textual input is highly dependent on the specificities of the latter. Here are some guidelines on the specific format of the texts to be provided: (1) each action must be described in a single sentence, (2) the succession of sentences marks the chronology of actions.


**Step 2: Generating the process image**

This is the second stage of the process, the aim being to start from a dictionary of steps, with all their precedents, and build a visual representation of the actions. To achieve this, we first had to define the type of object that the NLP part would provide. We started with a dictionary where keys represent actions and values represent the set of actions that preceded each key action. Here's an example:
``` 
{'A': [], 'B': ['A'], 'C': ['A'], 'D': ['B', 'C', 'A'], 'E': ['B', 'A'], 'F': ['D', 'A', 'C', 'B'], 'G': ['B', 'A', 'C', 'E', 'F']}
```
For task representation, the following sequence of actions was used:
- Identification of immediate precedents : For this, we referred to the PERT (Program Evaluation and Review Technique) task scheduling method in graph theory. It is particularly effective when there are multiple dependencies between tasks.  To arrive at a more or less correct representation, this method requires us to first determine the rank of the tasks, which makes it easier to determine the immediate antecedents. The rank of a task corresponds to its position in the logical sequence of tasks. More precisely, the rank of a task is determined by the highest value among the ranks of all the tasks directly preceding it. The starting rank is generally 0, since the first task does not precede any other task.  Once the ranks are known, we can extract the immediate antecedents of each task.
- Construction of the graph nodes and arcs : The graph can then be constructed from the list of immediate antecedents.

Output for the example described above: 
![image](https://github.com/user-attachments/assets/64ea79be-1f21-41d8-8c94-d89055407d8a)

## RESULTS
A text portion was used to showcase the results as shown below. 

Text :
```
Pour préparer le riz, il faut d'abord le rincer plusieurs fois à l'eau froide. Ensuite, verser le riz dans une casserole avec de l'eau froide et porter à ébullition. Réduire le feu et couvrir la casserole. Laisser cuire pendant environ 18 minutes. Enfin, retirer la casserole du feu et laisser reposer le riz pendant 5 minutes avant de servir.
```
Output : 

![image](https://github.com/user-attachments/assets/863a4413-bce7-4918-adda-05a5a25a6ad1)

