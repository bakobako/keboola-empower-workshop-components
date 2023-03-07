## Step 08: Develop the component code and UI

The component.py code in the source directory is the main part of the template.
It contains the main architecture of all components.
The class Component utilizes the Keboola Component library which provides a Python wrapper over the Keboola Common
Interface.
It simplifies all tasks related to the communication of the component with the Keboola Connection that is defined by the
Common Interface.
Such tasks are config manipulation, validation, component state, I/O handling, I/O metadata and manifest files, logging,
etc.

The main part of the component.py code is the run function,
this contains all the logic of your component; connecting to clients, pushing or downloading data to endpoints,
saving data to the output, etc. The run function is populated with common uses of functions like handling the state,
config, tables, manifests, and logging.

## Update the UI

The inputs of the app will be #api_token, base_prompt, text_column, all string inputs. View
the [UI documentation](https://developers.keboola.com/extend/component/ui-options/configuration-schema/) and
[examples](https://developers.keboola.com/extend/component/ui-options/configuration-schema/examples/) on how it works
and how to create your own. The final UI schema should be :

```json
{
  "type": "object",
  "title": "extractor configuration",
  "required": [
    "#api_token"
  ],
  "properties": {
    "#api_token": {
      "type": "string",
      "format": "password",
      "title": "OpenAI API Token",
      "propertyOrder": 1
    },
    "base_prompt": {
      "type": "string",
      "format": "textarea",
      "title": "Base Prompt",
      "propertyOrder": 2
    },
    "text_column": {
      "type": "string",
      "title": "Column Containing text",
      "propertyOrder": 3
    }
  }
}
```

## Update component.py with the script code

First import the necessary imports. Your final imports should look like :

```python
import logging
import json
import openai
from typing import Iterator, List, Dict
from csv import DictReader, DictWriter
from keboola.component.base import ComponentBase
from keboola.component.exceptions import UserException
```

Next define your Configuration keys

```python
KEY_API_TOKEN = '#api_token'
KEY_PROMPT = 'prompt'
KEY_TEXT_COLUMN = "text_column"
```

Set which parameters are required

```python
REQUIRED_PARAMETERS = [KEY_API_TOKEN, KEY_PROMPT]
```

Set the Model properties variables (these can later be set as configuration parameters)

```python
MODEL_NAME = "text-davinci-003"
MODEL_BASE_TEMPERATURE = 0.7
MODEL_BASE_MAX_TOKENS = 512
MODEL_BASE_TOP_P = 1
MODEL_BASE_FREQUENCY_PENALTY = 0
MODEL_BASE_PRESENCE_PENALTY = 0
```

Copy the code from the script

```python
def read_messages_from_file(file_name: str) -> Iterator[Dict]:
    with open(file_name) as in_file:
        yield from DictReader(in_file)


def process_message(openai_key: str, prompt: str) -> str:
    openai.api_key = openai_key
    response = openai.Completion.create(
        model=MODEL_NAME,
        prompt=prompt,
        temperature=MODEL_BASE_TEMPERATURE,
        max_tokens=MODEL_BASE_MAX_TOKENS,
        top_p=MODEL_BASE_TOP_P,
        frequency_penalty=MODEL_BASE_FREQUENCY_PENALTY,
        presence_penalty=MODEL_BASE_PRESENCE_PENALTY
    )
    return response.choices[0].text


def generate_prompt(base_prompt: str, message: str) -> str:
    return f"{base_prompt}\n\"\"\"{message}\"\"\""


def analyze_messages_in_file(in_file_name: str,
                             text_column: str,
                             out_file_name: str,
                             out_file_columns: List[str],
                             base_prompt: str,
                             openai_key: str) -> None:
    with open(out_file_name, 'w') as out_file:
        writer = DictWriter(out_file, out_file_columns)
        for message in read_messages_from_file(in_file_name):
            prompt = generate_prompt(base_prompt, message.get(text_column))
            data = json.loads(process_message(openai_key, prompt))
            writer.writerow({**message, "open_ai_output": data})
```

Clean up the code from the template, you will not need it, and start with the basic code

```python
class Component(ComponentBase):
    def __init__(self):
        super().__init__()

    def run(self):
        self.validate_configuration_parameters(REQUIRED_PARAMETERS)


if __name__ == "__main__":
    try:
        comp = Component()
        # this triggers the run method by default and is controlled by the configuration.action parameter
        comp.execute_action()
    except UserException as exc:
        logging.exception(exc)
        exit(1)
    except Exception as exc:
        logging.exception(exc)
        exit(2)
```

Now you can add all the necessary steps into the components **run function**

Fetch your parameters from the configuration
```python
params = self.configuration.parameters

base_prompt = params.get(KEY_PROMPT)
api_token = params.get(KEY_API_TOKEN)
text_column = params.get(KEY_TEXT_COLUMN)
```

Fetch your input table (any error handling i.e. if no tables are input can be handled later)
```python
input_table = self.get_input_tables_definitions()[0]
```

Determine the final output columns for the out_table writer. The input table definition contains the columns.
For testing use the data for local testing [available in this repo](https://github.com/bakobako/keboola-empower-workshop-components/tree/main/resources/data_for_local_componen_testing)
It contains a sample csv along with its manifest

```python
out_table_columns = input_table.columns + ["open_ai_output"]
```

Create an out table definition. Since we are defining the columns in the table definition, we have to make sure not to
write the headers to the csv file, because the headers will then show up in the data.

The options are write headers to file and dont write them to the manifest, or vice versa. It is recommended to use column in the manfifest.

```python
output_table = self.create_out_table_definition("analyzed_output.csv", columns=out_table_columns)
```

Now use the script to analyze data and save it to the output file.

```python
analyze_messages_in_file(input_table.full_path, text_column, output_table.full_path, out_table_columns, base_prompt, api_token)
```

Finally, use the table definition to write the manifest file for the output table.

```python
self.write_manifest(output_table)
```


Your final code should be :

```python
import logging
import json
import openai
from typing import Iterator, List, Dict
from csv import DictReader, DictWriter
from keboola.component.base import ComponentBase
from keboola.component.exceptions import UserException

KEY_API_TOKEN = '#api_token'
KEY_PROMPT = 'prompt'
KEY_TEXT_COLUMN = "text_column"

REQUIRED_PARAMETERS = [KEY_API_TOKEN, KEY_PROMPT, KEY_TEXT_COLUMN]

MODEL_NAME = "text-davinci-003"
MODEL_BASE_TEMPERATURE = 0.7
MODEL_BASE_MAX_TOKENS = 512
MODEL_BASE_TOP_P = 1
MODEL_BASE_FREQUENCY_PENALTY = 0
MODEL_BASE_PRESENCE_PENALTY = 0


def read_messages_from_file(file_name: str) -> Iterator[Dict]:
    with open(file_name) as in_file:
        yield from DictReader(in_file)


def process_message(openai_key: str, prompt: str) -> str:
    openai.api_key = openai_key
    response = openai.Completion.create(
        model=MODEL_NAME,
        prompt=prompt,
        temperature=MODEL_BASE_TEMPERATURE,
        max_tokens=MODEL_BASE_MAX_TOKENS,
        top_p=MODEL_BASE_TOP_P,
        frequency_penalty=MODEL_BASE_FREQUENCY_PENALTY,
        presence_penalty=MODEL_BASE_PRESENCE_PENALTY
    )
    return response.choices[0].text


def generate_prompt(base_prompt: str, message: str) -> str:
    return f"{base_prompt}\n\"\"\"{message}\"\"\""


def analyze_messages_in_file(in_file_name: str,
                             text_column: str,
                             out_file_name: str,
                             out_file_columns: List[str],
                             base_prompt: str,
                             openai_key: str) -> None:
    with open(out_file_name, 'w') as out_file:
        writer = DictWriter(out_file, out_file_columns)
        for message in read_messages_from_file(in_file_name):
            prompt = generate_prompt(base_prompt, message.get(text_column))
            data = json.loads(process_message(openai_key, prompt))
            writer.writerow({**message, "open_ai_output": data})


class Component(ComponentBase):
    def __init__(self):
        super().__init__()

    def run(self):
        self.validate_configuration_parameters(REQUIRED_PARAMETERS)
        params = self.configuration.parameters

        base_prompt = params.get(KEY_PROMPT)
        api_token = params.get(KEY_API_TOKEN)
        text_column = params.get(KEY_TEXT_COLUMN)

        input_table = self.get_input_tables_definitions()[0]

        out_table_columns = input_table.columns + ["open_ai_output"]

        output_table = self.create_out_table_definition("analyzed_output.csv", columns=out_table_columns)

        analyze_messages_in_file(input_table.full_path, text_column, output_table.full_path, out_table_columns,
                                 base_prompt, api_token)

        self.write_manifest(output_table)


if __name__ == "__main__":
    try:
        comp = Component()
        # this triggers the run method by default and is controlled by the configuration.action parameter
        comp.execute_action()
    except UserException as exc:
        logging.exception(exc)
        exit(1)
    except Exception as exc:
        logging.exception(exc)
        exit(2)

```

## Update requirements.txt

Add the openai library to the requirements.txt

[Next Step](https://github.com/bakobako/keboola-empower-workshop-components/blob/main/workshop_steps/Step%2009%3A%20Test%20component%20locally.md)