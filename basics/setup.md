# Introduction
This folder contains simple, self-contained notebooks and scripts that cover common activities I encounter as a data scientist.

# Instructions
For simplicity, I'm assuming you're using python3 and are familiar with virtualenv. I'm not an Anaconda user, so this is mostly written around using pip and virtualenv.

## Setup a venv
I manage my virtualenvs in a ~/.virtualenvs directory. Adapt to where you store yours as needed. 
Furthermore, I've decided to have single virtualenvs for each high-level folder. So this `basics` folder has a single virtualenv that all the notebooks, scripts, and code in this folder make use of.

Setup the `basics` virtualenv
`python3 -m venv ~/.virtualenvs/basics`

Activate it
`source ~/.virtualenvs/basics/bin/activate`

Install the requirements
`pip3 install -r requirements.txt`
