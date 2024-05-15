Github Actions
==============

At this point, please make your own fork of the repository so you can follow along with the hands-on portion of the demo. You only need to copy the main branch (see figure below):


    .. figure:: ../_static/img/ForkingRepo.png

        A screenshot of forking the repo.

#. **What Are Github Actions?**

    Github Actions are workflows that can be automatically triggered. Examples include:

    * Checking code quality.
    * Running automated tests.
    * Building documentation/other websites.
    * Building code artifacts/docker images.

#. **Code Quality Check with Github Actions**

    We can use an existing job ``flake8-lint`` to run a simple Github Action that will automatically check the code quality of our work going forward!

    We will be using the simple action below. You can name an action whatever you want and we can specify which actions we would like to trigger our job. In our case, all pushes and pull requests will trigger the job which is convenient to ensure that new submissions maintain our code quality.

    Then, we specify the jobs to run. Our only job here is named ``flake8-lint``. This job runs on an ubuntu-latest "runner" (a virtual machine specified by Github) and we can provide the job with a name/label as well. Then, we specify the step in our job:

    #. Check out the repo (we need the code to check its quality!). `The checkout action is here <https://github.com/actions/checkout>`_.
    #. Setup the Python environment (using the ``requirements.txt``). `The setup-python action is here <https://github.com/actions/setup-python>`_.
    #. Run our flake8 check. `The flake8 action is here <https://github.com/py-actions/flake8>`_.

    .. code-block:: yaml

        name: Python Code Quality

        on: [push, pull_request]

        jobs:
            flake8-lint:
                runs-on: ubuntu-latest
                name: Python Code Quality
                steps:
                - name: Check out source repository
                uses: actions/checkout@v2

                - name: Set up Python environment
                uses: actions/setup-python@v2
                with:
                    python-version: "3.x"

                - name: flake8 Lint Check
                uses: py-actions/flake8@v1
                with:
                    path: "mycybergis"
                    args: "--ignore=E501,E722,F403,F405"

#. **Running Our Action**

    Copy and paste the action from the yaml code block above. On the Github webpage for your fork, click "Add file" in the top-right, then select "Create new file" (you can also add this other ways).


    .. figure:: ../_static/img/CreateNewFile.png

        A screenshot ofcreating a new file.


    Paste the yaml code block into the editor and name your file ``.github/workflows/CodeQuality.yaml``. By placing this yaml file in ``.github/workflows``, Github will recognize the file as an Action.

    Commit your changes and go to the Actions tab on your repo to see the action being run!

#. **Checking On Your Action**

    On the Actions tab you should be able to see the results of your Action's run.


    .. figure:: ../_static/img/ActionResults.png

        A screenshot of Action results.

    If you click on the Action you can get further details on the Action including all outputs, warnings, duration, etc.

#. **Action for Pytest**

    Here is a yaml for running Pytest automatically. Try adding it to your repo

    .. code-block:: yaml

        name: Pytest
        on: [push]
        jobs:
            build:
                runs-on: ubuntu-latest
                strategy:
                    matrix:
                        python-version: [ 3.12.3 ]
                steps:
                - uses: actions/checkout@v2
                
                - name: Set up Python
                    uses: actions/setup-python@v2
                    with:
                    python-version: ${{ matrix.python-version }}
                
                - name: Install dependencies
                    run: |
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                
                - name: Test with pytest
                    run: |
                    pip install pytest pytest-cov
                    pytest --cov-report=term --cov=mycybergis -s test
                    coverage report


#. **More Actions!**

    There are a wide variety of actions to help you maintain working and well-formatted code. Explore `the Github Actions Marketplace to see the options <https://github.com/marketplace>`_.