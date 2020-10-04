# Setup a venv
I manage my virtualenvs in a ~/.virtualenvs directory. Adapt to where you store yours as needed. 
Furthermore, I've decided to have single virtualenvs for each high-level folder. So this `basics` folder has a single virtualenv that all the notebooks, scripts, and code in this folder make use of.

Setup the `basics` virtualenv
`python -m venv ~/.virtualenvs/basics`

Activate it
`source ~/.virtualenvs/basics/bin/activate`

Upgrade pip
`pip install --upgrade pip`

Install the requirements
`pip install -r requirements.txt`

When you're done using your virtualenv, then just enter `deactivate` to return to your default system environment.

