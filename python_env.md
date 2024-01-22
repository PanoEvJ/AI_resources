
# Step-by-Step Instructions: Running Firebase Functions Locally within a Python Environment

This step-by-step guide provides detailed instructions on how to run Firebase functions locally using a 
Python environment.

## Initial Git Operations

<span style="color:red">
Current directory
</span>
: any directory in ./noa-chat/ 

<br>

1. Fetch the latest changes from the Git repository and merge content from `main` branch: 
    ```
    git fetch --all
    git pull origin main
    ```
   This updates your local repository to match the `main` branch on the remote repository. 

## Managing the Python environment


   <span style="color:red">
   Current directory
   </span>
   : noa-chat/cloud-functions/python/ 

   <br>

2. Install the dependencies & Activate the environment:

   **1st alternative**: Using **Conda** to manage the virtual environment and **Poetry** to manage the python packages:  


      ```
      conda activate my_conda_env
      ```
      ```
      poetry install
      ```

      Activate the Poetry package manager:

      ```
      source $(poetry env info --path)/bin/activate

      # or 

      source /home/username/.cache/pypoetry/virtualenvs/noa-xQILWGgO-py3.11/bin/activate
      ```

   **2nd alternative**: Using **Poetry** to manage both the virtual environment and the python packages:

      ```
      poetry install
      ```

      Activate the Poetry virtual environment:

      ```
      poetry shell
      ```
   
## Create an environment requirements file

<span style="color:red">
Current directory
</span>
: noa-chat/cloud-functions/python/ 

<br>

3. Export the Poetry requirements to a file: 

    ```
    poetry export -f requirements.txt --output requirements.txt
   ```

   This step is required by our setup to run Firebase locally.

## Creation and Verification of New Python Virtual Environment

<span style="color:red">
Current directory
</span>
: noa-chat/functions/python/ 

<br>

4. Remove any pre-existing venv directory:

   ```
   rimraf venv
   ```

5. Create a symbolic link in the current directory, pointing to the new Poetry environment for easier access:

   ```
   ln -s $(poetry env info --path) venv

   # or

   ln -s /home/username/.cache/pypoetry/virtualenvs/noa-xQILWGgO-py3.11/bin/activate venv
   ```

   Make sure that all symbolic links are setup:

   ```
   ll
   ```

   - Should look include the following:

      > firestore-data/

      > poetry.lock -> ../../cloud-functions/python/poetry.lock

      > pyproject.toml -> ../../cloud-functions/python/pyproject.toml

      > requirements.txt -> ../../cloud-functions/python/requirements.txt

      > venv -> /home/username/.cache/pypoetry/virtualenvs/noa-Dnver8q3-py3.11/

## Connecting the application to the emulators
<span style="color:red">
Current directory
</span>
: noa-chat/functions/python

<br>

   6. Once the emulators have started, you will need to let the front end know that it should now use the emulator. To do this, you will need to create a .env.local file in the root directory of the project. This file should contain the following:

      ```
      VITE_FIREBASE_FIRESTORE_EMULATOR_ENABLED=true
      ```


## Starting the Firebase Emulator

<span style="color:red">
Current directory
</span>
: noa-chat/functions/python/ 

<br>

7. Start Firebase emulator with the specified Firestore data and other settings:
   ```
   firebase emulators:start --project noa-dev-dbb6 --import firestore-data --export-on-exit ./firestore-data
   ```

   This command starts the Firebase emulator and configures it to use specific project and Firestore data settings.
   
   If you get an error that a ```firestore-data``` directory does not exist, then create one
   
   ```
   mkdir firestore-data
   ```
   
   and repeat the previous command.


   This method provides a fast and straightforward way to set up an isolated environment for your Python project. It ensures that all Python dependencies are compartmentalized within this environment, avoiding version conflicts with other projects.

## Starting the application

<span style="color:red">
Current directory
</span>
: any directory in ./noa-chat/ 

<br>

8. The web runs on port 3000 by default. To start the application, open a new terminal and run:
   
   ```
   yarn dev
   ```

## Seeding
<span style="color:red">
Current directory
</span>
: any directory in ./noa-chat/ 

<br>

   9. For Noa to run correctly locally, each time the firestore emulator is started you will need to seed the departments into the firestore emulator. To do this, run the following command from the root directory:

      ```
      yarn seed:local
      ```


   
# Some useful Poetry commands
(not neccessary for this step)

- Check the current Python virtual environment path for Poetry: 
   ```
   poetry env info --path
   ```

   This displays the path to the current Python virtual environment being used by Poetry. It should look something like this:

   > /home/username/.cache/pypoetry/virtualenvs/noa-m6ogbQDh-py3.11

- List of poetry environments:
   ```
   poetry env list

   # or

   ls /home/username/.cache/pypoetry/virtualenvs
   ```

- De-activating the current poetry env:

   ```
   deactivate
   ```
   
  Re-activating a selected poetry environment:
   ```
   source /home/username/.cache/pypoetry/virtualenvs/noa-xQILWGgO-py3.11/bin/activate
   ```

- To activate a Poetry-managed virtual environment:
   ```
   cd noa-chat/cloud-functions/python/
   poetry install
   poetry shell
   ```

   Exit the virtual environment by:
   ```
   poetry exit
   ````