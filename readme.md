testing phone networks with GSM modules


### requirements
* one or more eno hardware nodes (Beaglebone Black + a Fona module)
* Python 2.7


### installation

```shell
$ pip install eno
```


### usage
A cluster of eno hardware nodes are setup
with network connectivity to a testing machine.
The testing machine uses `eno.nodes.Node` to control the connected nodes
and to read data from them.
The nodes themselves run the `eno.server.app`.
Other clients (like the testing machine) connect to this server
to give instructions to the hardware and to read data back.
The hardware uses [python-gsmmodem](https://github.com/faucamp/python-gsmmodem)
to communicate with the onboard GSM modem.

The testing machine should have an `~/.enorc` describing the test cluster:

```yaml
- name: node A
  ip_address: 192.168.1.102
  sim: endaga
- name: node B
  ip_address: 192.168.1.105
  sim: endaga
- name: node C
  ip_address: 192.168.1.107
  sim: ting
  phone_number: 19195551234
```

See additional examples in `server.py` and `sample_test.py`.


### testing concurrent calls
By default, when making a call, the test server
will make the modem hang up a few seconds after the call connects.
I experimented with a `/hangup` endpoint but that complicates things --
we would need signals and some WSGI runner
in order to have more than one server process.
So instead you can specify `hangup_after` as a request param,
this will hangup the call after the specified number of seconds.


### license
MIT


### releases
* 0.0.17 - adjusts return values in `server.call`
* 0.0.16 - fixes modem definition
* 0.0.15 - adds call functionality
* 0.0.14 - adds more request timeouts and `Node.get_info`
* 0.0.13 - adjusts handling of `Node.wait_for_activity`
* 0.0.12 - adds protocol in `Node.server_address`
* 0.0.11 - fixes port default
* 0.0.10 - allows tilde expansion when searching for enorc config file
* 0.0.9 - adds `Node.wait_for_activity` for sms
* 0.0.8 - adjusts handling of sms log in Node class
* 0.0.7 - changes sms log deletion method
* 0.0.6 - improves error handling and modem connection management
* 0.0.5 - adjusts package structure
* 0.0.4 - adds missing requests requirement
* 0.0.3 - adjusts server script filename
* 0.0.2 - adds SMS handling capabilities
* 0.0.1 - barebones setup for pypi


### release process
you need a `~/.pypirc` like this:

```
[distutils]
index-servers =
  pypi

[pypi]
repository: https://pypi.python.org/pypi
username: yosemitebandit
password: mhm
```

bump the versions in `setup.py` and here in the readme, then run:

```shell
$ git tag 0.0.1 -m 'eno-python v0.0.1'
$ git push origin master --tags
$ python setup.py sdist upload -r pypi
```
