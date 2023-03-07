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

## Update component.py with the script code

View the script

## Update requirements.txt

[Next Step](https://github.com/bakobako/keboola-empower-workshop-components/blob/main/workshop_steps/Step%2009%3A%20Test%20component%20locally.md)