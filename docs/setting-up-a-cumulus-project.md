
# Notes on setting up GIT, SFDX, and Cumulus

## Installing what you need

1. [Install Visual Studio Code](https://code.visualstudio.com/download)

2. [Install Git](https://git-scm.com/downloads)
    Make sure to choose:

    * Use Git and optional Unix tools from the Windows Command Prompt
    * Also, when prompted for a editor choose Visual Studio Code

3. [Install Salesforce CLI - SFDX ](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm)

4. [Install Salesforce CLI extensions in Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)

5. [Install Python](https://www.python.org/downloads/windows/)
    * When installing Python do not express install and make sure to select these options:
        * Install for all users
        * Set Environmental Paths
        * Install in the root of your C: drive.
    * Open Command Prompt from start
    * Enter the following into your command line:

        ```bash
        python -m pip install --user pipx
        ```

    * Set your windows Environmental Variables

        * Go to start typing "Environmental." When "Edit the System Environmental Variables" appears choose it.
        * Click the "Environmental Variables" button near the bottom of the dialog.
        * In the variables for your user name click on the "Path" variable and then click "Edit..." below.
        * Click "New" and add this path to the highlighted spot:

        ```bash
        %USERPROFILE%\AppData\Roaming\Python\Python37\Scripts
        ```

        * Click "New" again and add this path to the highlighted spot:

        ```bash
        %USERPROFILE%\.local\bin
        ```
6. [Install Cumulus](https://cumulusci.readthedocs.io/en/latest/install.html#installing-cumulusci)
    * Open Cmd.exe from start
    * Enter the following into your command line:

        ```bash
        pipx install cumulusci
        ```
