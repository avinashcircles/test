# Circles Configuration Manager


## What is ETCD?

Etcd is a distributed key value store that provides a reliable way to store data across a cluster of machines.   The applications can read and write data into etcd. A simple use-case is to store database connection details or feature flags in etcd as key value pairs. These values can be watched, allowing your app to reconfigure itself when they change.


## What is Circles Configuration Manager

The CCM library is an adapter over existing etcd clients available in various technologies.

	- The CCM client allows get() and set() calls for fetching and setting the configuration values. For the first time it gets all the Key-Value pairs present under a directory in ETCD and caches it in local cache. For all subsequent calls it will fetch it from the local cache.
	- CCM supports watcher which will update the local key:value at the path in local cache.In case of any new key addition or existing key modification under a particular directory, the watcher notifies and the changes are synced up with the local cache.
	- CCM also support inheritance of configuration. This will allow a configuration defined at parent level will actually be available at any child scope unless it's overridden by another value.
	- CCM will re-fetch the configuration key value if it is accessed after the expiry ts. This is important to keep the configurations latest even in case of network interruptions.
## SETUP:


1. Create a go folder where all go projects will reside:
	- The folder will look somewhat like this ~/go
	```
			mkdir go
	```
	-  Under the go folder create three sub folders /src, /pkg and /bin
	```
			cd go
			mkdir src pkg bin
	```
	-  Create folder bitbucket.org/liberywireless inside /src
	```
			cd src
			mkdir bitbucket.org
			cd bitbucket.org
			mkdir libertywireless
	```
2. Inside the libertywireless directory clone the repository circles-go using the following commands
	-  Traverse to the libertywireless directory
	```
			cd libertywireless
	```
	-   Clone the package using git clone command.
	```
			git clone git@bitbucket.org:libertywireless/circles-go.git
	```
	-  Run go get
	```
			cd bitbucket.org/libertywireless/circles-go
			go get 
	```
