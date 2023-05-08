# Pupil Labs Core

Here are some set up notes for the Pupil labs core. They require to install interfaces for zeromq and msgpack. For zeromq you can clone this repo and add the lib folder to the MATLAB path.

https://github.com/iandol/matlab-zmq

For msgpack, the two files from https://github.com/bastibe/matlab-msgpack are already included in opticka.

To install the pupil core software, you need Python. I clone the repo https://github.com/pupil-labs/pupil and also install Python 3.11 wih `pyenv` and then create a virtual env to run the pupil software

```
cd ~/Code/pupil
pyenv install 3.11.3
pyenv global 3.11.3
python -m venv ~/.venv/pupilcore
pya pupilcore
pip install --upgrade pip
python -m pip install -r requirements.txt
python pupil_src/main.py capture
```

