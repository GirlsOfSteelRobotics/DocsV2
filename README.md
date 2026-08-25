
Documentation and tutorials for the Girls of Steel (FRC 3504) robotics team.

Rendered docs are available at https://girlsofsteeldocs.readthedocs.io/en/latest/


## Building docs locally

### One time setup
Create a virtual environment and install required software

```
python3 -m venv .venv

source .venv/bin/activate

pip install -r requirements.txt 
```


### Actually building
```
sphinx-build -M html source build
```