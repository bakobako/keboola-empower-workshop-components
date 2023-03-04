## Step 2: Look through the python script

The [Python script](https://github.com/bakobako/OpenAI-Data-Analyzer) is a simple piece of code that uses the OpenAI 
API to analyze text. The input is a base prompt, API token, and a csv file. It goes through a CSV file containing text 
to be analyzed and for each row selects the message column and analyzes the text in that column. The output gets stored 
in an output csv with the same data as the input table, but with an extra column "open_ai_output" containing the response 
of the OpenAI model.

### * Base Prompt
The base prompt contains the instructions to the API, it gets combined with the message to form the prompt for the OpenAI Model, 
e.g. base prompt is "Is the following text positive or negative?" and the message is "I like you", the final prompt would be:

Is the following text positive or negative?

"""
I like you
"""

### * OpenAI API Token

The OpenAI API token can be generated [here](https://platform.openai.com/) click on Personal, 
and select View API keys in drop-down menu. You can then copy the key by clicking on the green text Copy.

### * CSV File

The csv file should contain a column that contains text that should be analyzed.