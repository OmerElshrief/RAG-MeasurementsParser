# BASF Measurements Parser

Chemical Research patents Measurements Parser.

## Overview
LLM based application that parses Chemical research patents to extract Measurements and their units. The application is based uses GPT3.5 and Langchain to parse Patents and process inputs and outputs.

### GOAL:
Parse large text and extract unstructured Measurements in a structured format.
### Features:
1- Parse large XML files that contains many patents and extract useful text.
2- Pre-process Text and format it in a way that fits a LLM.
3- Uses Langchain to send large files to GPT3.5.
4- Post-process output to be in a structured format (JSON).
5- Compare and evaluates prompts.

### LLM
Mainly uses GPT3.5 and Langchain to communicate with the API and process inputs.

### Prompt Engineering
We set a guideline for writing prompts, the application is configurable and will work with any prompts.

#### Adding a new prompt
In the prompt directory from the main directory, there will be all prompts, each prompt in a directory with name as the prompt ID.


Provide an overview of your project, explaining its purpose, goal, and main features.

## Table of Contents

- [BASF Measurements Parser](#basf-measurements-parser)
  - [Overview](#overview)
    - [GOAL:](#goal)
    - [Features:](#features)
    - [LLM](#llm)
    - [Prompt Engineering](#prompt-engineering)
      - [Adding a new prompt](#adding-a-new-prompt)
  - [Table of Contents](#table-of-contents)
  - [Installation](#installation)
    - [Requirements](#requirements)
    - [prerequisites](#prerequisites)
      - [Prompts Directory](#prompts-directory)
  - [Usage](#usage)
    - [Data Preparation:](#data-preparation)
    - [Prompt Building and Engineering.](#prompt-building-and-engineering)
    - [Prompt Evaluation](#prompt-evaluation)
      - [Context Evaluation](#context-evaluation)
  - [Examples and Demos](#examples-and-demos)

## Installation

### Requirements
To install requirements, 'pip install -r requirements.txt'
### prerequisites
Since the application mainly uses LLM, main building block is a Prompt. Make sure there is at least one prompt in the Prompts directory.

#### Prompts Directory
Prompts Directory contains a sub directory for each prompt with the prompt id as the subdirectory name. each subdirectory contains mainly 2 files:
1- prompt.json: This file contains prompt instructions and output format, ex: 

```
{
 "text": "You are given a text from chemistry research patent, extract all the measurements mentioned in the text. Extract Measurements Details, Unit, Value or Value ranges.",

 "output_format": "Your output should follow be a list of json objects, good output format : [ { 'measurement ' : 'string ', 'unit ' : 'string ', 'value ' : 'string ' }, { 'measurement ' : 'string ', 'unit ' : 'string ', 'value ' : 'string ' }", "id": "000"
}
```

2- examples.json: Contains examples used with the prompts (in case few-shots prompting). The examples should be in a list of json objects, ex:
```
[{"text": "text to be parsed",  "measurements": [ { "measurement": "polypeptide concentration", "unit": "g/ml", "value": "1 to 9" } ]}]
```

To add a prompt, create a new directory and add both files and make sure they follow the same format mentioned above.


Explain the steps required to install and set up your software. Include any prerequisites or dependencies needed. Provide clear instructions for configuring the environment.

## Usage
### Data Preparation:
It is assumed that the given file to parse is an XML file that contains many patents, to parse the file and split patents, use the **Data.ipynb** notebook, it does the following:
1- Read xml file and split patents.
2- Filter patents to keep only Chemistry patents.
3- Extract text from <abstract>, <claims>, <description> elements and keep the text for all patents in a txt file.

Follow the **Data.ipynb** notebook for data preparation.

### Prompt Building and Engineering.
Prompt building is a crucial step, as mentioned in (#prompts-directory), you need to have your prompt directory prepared.
We provide some prompt design principles that could help writing prompts.

 • **Be clear and specific**: The prompt should clearly state the task or question that the model is expected to answer. Avoid ambiguity or vagueness in the prompt as this can lead to unclear or irrelevant responses.
 • **Provide context**: The prompt should provide enough context for the model to understand the task and generate a relevant response. This can include relevant background information, examples, or constraints.
 • **Use natural language**: Write the prompt in a natural and conversational style that the model can understand and respond to. Avoid using complex or technical language that may confuse the model.
 • **Be concise**: Keep the prompt concise and to the point. Avoid unnecessary details or information that may distract the model from the task at hand.
 • **Avoid complex sentence structure:** Using complex sentence structures can confuse the model, leading to poorly generated output. Using simple sentences and straightforward language makes it easier for the model to understand and generate the desired output.
 • **Avoid ambiguity:** Ensure that the prompt is specific and avoids ambiguity. Avoid using words with multiple meanings or phrases that can be interpreted in different ways.
 • Use keywords: Use keywords in the prompt that are relevant to the topic of the conversation. This helps the chatbot to understand the context and respond appropriately.
 • **Consider the intended audience:** Consider the intended audience for the generated responses and tailor the prompts accordingly. Use appropriate language, terminology, and examples that the audience is likely to understand and relate to.
 • **Use appropriate formatting:** Use appropriate formatting such as bullet points, numbered lists, or bold text to highlight key information in the prompt. This can help the model understand the structure and organization of the prompt.
 • **Test and refine:** Test the prompts with the model and refine them based on the quality of the generated responses. Iteratively refine the prompts until the generated responses are of high quality and relevance.
 • We should describe the task requirement in details, trying to be specific and precise.
 • Avoid saying “not do something” but rather specify what to do.
 • Avoid add unnecessary words, try to only add words that are relevant to the problem you are trying to solve.
 ○ **Instructions** is crucial as it guides the model on what to do and what is expected of it. It is important to be clear, concise and specific. When dealing with complex reasoning tasks, consider breaking the tasks down into smaller, more manageable steps to help the model understand the task at hand.
 • **Constraint** is helpful as guiding LLMs on what it can and cannot do, ultimately leading to more accurate results. This may involve specifying the format of the output, the type of language to be used, or even the length of the output. Additionally, specifying the difficulty level and style can further refine the output.

### Prompt Evaluation
#### Context Evaluation
For the prompt Evaluation, we need to put into consideration 3 main questions:
**Does the LLM (GPT) understand the provided context and examples (in case we provided any examples)?**
 1- We use the examples in the prompts as inputs, and check whether GPT outputs the expected output or no.
 2- If GPT outputs the expected output, then everything is good for this part.
 3- If the output is different, it's is probably because of the prompt is not clear, and It needs to be rewritten.

For Each prompt directory, there should be **Examples.json** file, this one is used in Prompt Context Evaluation.

**After we make sure that GPT understand our prompts and our Examples, how can we make sure GPT is not overfitting to the provided examples?**
There will be a separate test set with carefully crafted Ground truth labels to evaluate the prompt. 
Note that, this evaluation is not meant only for the Prompt, but also for the preprocessing and postprocessing steps. It's mandatory to choose the test samples carefully to make sure they cover almost all possible scenarios.

In the Data Directory, there is a test_set.json file that contains test examples, you can add more examples or remove examples.

For Prompt Evaluation and Building, follow the notebook **Prompts_Evaluation.ipynb**

Explain how to use your software. Provide examples or sample commands to demonstrate its usage. Describe the expected inputs and outputs. Include any necessary instructions for customizing or configuring the software.


## Examples and Demos

**Full Example.ipynb** Notebook contain a full pipeline example with discussion and results.
**Prompts Evaluation.ipynb** Notebook contains steps for evaluating prompts in the prompts directory with results and discussion.
**Parsing a text file.ipynb** Notebook contains steps to parse a text file and building the prompt for it with results and discussion.

