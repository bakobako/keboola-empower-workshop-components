# Empower Workshop : Component Creation

# Important Links

* [Slides used in the workshop](https://github.com/bakobako/keboola-empower-workshop-components/blob/main/resources/slides/empower_components_slides.pdf)
* [Python script that will turn into a component](https://github.com/bakobako/OpenAI-Data-Analyzer)
* [Final code of the component built in the workshop](https://github.com/bakobako/keboola-component-factory-demo.app-openai-workshop-prep)
* [Streamlit app shown in demo](https://github.com/bakobako/Sentiment-Streamlit)

# Workshop steps

## Step 1: Review the Slides

* Understand the [Developer Portal](https://components.keboola.com/)
* Understand the [Common Interface](https://developers.keboola.com/extend/common-interface/)
* Understand the purpose of the [Component Library](https://github.com/keboola/python-component)
* Understand the purpose of
  the [Component Template](https://bitbucket.org/kds_consulting_team/cookiecutter-python-component/src/master/?search_id=8a6f3c24-3f05-420a-8ec3-5d71cb084024)

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

## Step 3: Register a component in the Developer Portal

## Step 4: Create a component repository in GitHub

## Step 5: Setup CI variables in GitHub

## Step 6: Initialize Python component template with cookiecutter

## Step 7: Push template to repository and deploy it to Keboola

## Step 8: Develop the component code and UI

## Step 9: Test component locally

## Step 10: Deploy component code to Developer Portal

## Step 11: Run component in Keboola

