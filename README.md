# assignment
----------------------------------------------------------------------------
Problem Resolution with LevelDB in OpenTimestamps
## Context
During the setup of the OpenTimestamps server, compatibility issues arose between the leveldb
library and modern Python versions, which caused errors when initializing the LevelDB database.
## Problem
The error identified when trying to run the server was:
leveldb.LevelDBError: IO error: lock /home/<user>/.otsd/backups/db/LOCK: Resource temporarily
unavailable
Other versions of Python were also incompatible with the library.
## Applied Solution
It was determined that Python 3.7.12 is a stable version compatible with the leveldb library. Below
are the steps to correctly configure the environment.
### 1. Install the Correct Python Version
We used pyenv to install and activate the required Python version:
pyenv install 3.7.12
pyenv virtualenv 3.7.12 opentimestamps-env
pyenv activate opentimestamps-env
### 2. Reinstall Dependencies
With the virtual environment active, we installed the necessary dependencies for the
OpenTimestamps server:
pip install -r requirements.txt
pip install plyvel
### 3. Initialize the Server
With the environment set up, we started the server to confirm that the LevelDB database works
correctly:
python3 otsd-backup.py
If the server starts correctly and shows a message like the following, the issue is resolved:
db dir is /home/<user>/.otsd/backups/db
Starting at localhost:14799
## Verification
To verify that everything works properly, you can perform a test timestamping:
echo "OpenTimestamps Test" > test.txt
ots stamp -c http://127.0.0.1:14799 -m 1 test.txt
## Additional Notes
- Make sure you have the proper permissions for the LevelDB lock file:
 rm -f ~/.otsd/backups/db/LOCK
- If the problem persists, ensure no previous processes are occupying the required resources.

- -----------------------------------------------

With this solution, the OpenTimestamps server should be operating correctly.
