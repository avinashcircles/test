# Circles Configuration Manager


## What is ETCD?

Etcd is a distributed key value store that provides a reliable way to store data across a cluster of machines.   The applications can read and write data into etcd. A simple use-case is to store database connection details or feature flags in etcd as key value pairs. These values can be watched, allowing your app to reconfigure itself when they change.


## What is Circles Configuration Manager

The CCM library is an adapter over existing etcd clients available in various technologies:

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
	
	
	
	
## HOW TO USE THE LIBRARY ?

### Importing the library:

-  Import the library using the following command inside the import area of your code
```
	import “~/…/go/src/bitbucket.org/libertywireless/circle-go/ccm_client"
```
### Set up the environment variables:

-  There are a set of environment variables that has to be set up before initialising. The library uses these environment variables creates the connection between the etcd server and the client

-  ETCD_ENDPOINTS gives all the IP address and port names of the etcd servers.
```
	export ETCD_ENDPOINTS=“10.30.1.65:2379,10.30.1.66:2379,10.30.1.67:2379”
```
-  To specify the username and password for the particular folder access in etcd
```
	export ETCD_USERNAME=“rewards”
	export ETCD_PASSWORD=“Corresponding_password”
```
-  The following environment variable specifies the cache expiration time in secs
```
	export CCM_CACHE_TTL=“100”
```

-  The watcher path variable ensures the monitoring of the folder under which the key:values are stored
```
	export CCM_APP_NAME=rewards
	export CCM_ETCD_WATCHER_PATH=“/base_path_of_the_folder”
```

If any one of the environment variable is not set, the program throws an error. No default values are provided internally.

### Initialise the library:

-  Once the environment variables are set, we have to initialise the library

-  Initialise a CcmConfig struct variable ccmConfig 
```
	ccmConfig = new(CcmConfig)
	ccmConfig = {AppConfig:generalConfig}
```
		
The generalConfig must have a Set method for reloading the corresponding values in the application.
		
-  Initialise the library
```
	ccm_client.Init(&ccmConfig)
```

- This initialises the library and establishes a connection to the etcd server.

### Get Value for a particular Key:

-  This function has two arguments
	- Key : the value at the end of the path.
	- Scope : the path excluding the base path and key

-  Example: 
	- Complete path: /rewards/tw/silver/roaming_cap
	- Base path : /rewards
	- Scope : /tw/silver
	- Key : roaming_cap

-  To get a value of a particular key
```
	ccm_client.Get( Key, Scope)
```

### Set Value for a particular Key:

This is a hidden method which requires EnableSet variable to be true for the method to execute its intended operation. The EnableSet variable is a part of the ccmConfig structure. 

-  To set a value for a particular key
```	
	ccm_client.Set( Key, Value)
```	
	
	
	
