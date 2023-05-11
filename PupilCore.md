# Pupil Labs Core

Here are some set up notes for the Pupil labs core. They require to install interfaces for zeromq and msgpack. For zeromq you can clone this repo and add the lib folder to the MATLAB path (I've added the compiled mex files to the repo so no need to compile):

https://github.com/iandol/matlab-zmq

For msgpack, the two files from https://github.com/bastibe/matlab-msgpack are now included in opticka.

To install the latest version of the pupil core software, you need Python (the app/exe/dmg are old from 2021). Clone the repo https://github.com/pupil-labs/pupil and also install Python 3.11 wih `pyenv` and then create a virtual env to run the pupil software

```
cd ~/Code/
git clone https://github.com/pupil-labs/pupil
cd pupil
pyenv install 3.11.3
pyenv global 3.11.3
python -m venv ~/.venv/pupilcore
pya pupilcore
pip install --upgrade pip
python -m pip install -r requirements.txt
python pupil_src/main.py capture
```
Note I use my own elvish commands like `pya` to activate the `venv`, if you are not using the `elvish` shell you must use the equivalent for `zsh` or `bash`.

# How to talk to the Core

The headset uses 3 interfaces. The first is a rep-req zeromq interface by dfault at port 50020. You connect to this using zmq:

```matlab
me.endpoint = ['tcp://' me.calibration.ip ':' str2num(me.calibration.port)];
me.ctx = zmq.core.ctx_new();
me.socket = zmq.core.socket(me.ctx, 'ZMQ_REQ');
zmq.core.setsockopt(me.socket, 'ZMQ_RCVTIMEO', me.calibration.timeout);
fprintf('--->>> pupilLabsManager: Connecting to %s\n', me.endpoint);
err = zmq.core.connect(me.socket, me.endpoint);
```

Once this is open you can then ask for ports for subscribe and publish so you can get gaze data and send time-locked markers to the recording system.

