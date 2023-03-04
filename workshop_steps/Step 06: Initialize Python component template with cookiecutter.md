## Step 6: Initialize Python component template with cookiecutter

**Note**
Make sure you have Python 3 and pip installed properly with "python --version" returning a version of Python 3 and 
"pip --version" also returning a version compatible with Python 3.

First install CookieCutter from your terminal using the command:

```
pip install cookiecutter
```

Open your terminal in the directory where you want to store your components locally, and run the CookieCutter script :

```
cookiecutter bb:kds_consulting_team/cookiecutter-python-component.git
```

or if you run it with python then :

```
python -m cookiecutter bb:kds_consulting_team/cookiecutter-python-component.git
```

CookieCutter will then require you to fill in some details about the component:

* Select template variant: Choose GitHub by entering “1”
* Enter the repository URL, e.g. https://github.com/github_user/my-vendor.ex-demo-component
* Enter the component name , e.g. My Demo Component
* Enter the repository folder name, it is best to use the component id, e.g. my-vendor.ex-demo-component
* Enter the component short description; describing the service you are building the component to integrate.
* Enter the component long description; describing the component and its use.
* Enter whether you wish to use flake8 checks for deployment. It’s good to have this turned on to maintain the quality of code.
* Enter whether you want to push a test tag once a branch is updated; this is useful for running a testing version of the component in Keboola without updating the live version of the component.

Once you finish the CookieCutter process you will have a customised template in the directory you had your terminal open in.