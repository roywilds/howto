# Keras
A collection of utilities and notebooks to assist in understanding how to use Keras. 

Relies heavily on the fantastic work by Arden Dertat: https://github.com/ardendertat/Applied-Deep-Learning-with-Keras 
Check out his Medium articles too.

## Setup
Create a virtualenv and install the requirements.

```bash
$ python -m venv ~/.virtualenvs/keras
```

I find it handy to soft-link from in my project to my venv's activate location because I keep my venv folders in a different location than the code.

```
$ ln -s ~/.virtualenvs/keras/bin/activate venv_activate
```

With this done, then you can activate your `keras` venv via:

```
$ . venv_activate

(keras)
$ pip install -r requirements.txt
```

# Notebooks
Once you're setup and have your venv active, then start the Jupyter Notebook server.

```bash
$ jupyter notebook
```

## Autoencoder
The `autoencoders.ipynb` notebook walks through creating and understanding an autoencoder. 
A great introduction to ANNs and Keras for an unsupervised ML problem.

