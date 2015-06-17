Hello World Application
========================

1. Download the Ninja Sphere Go library

	* `Ninja Sphere Go library <https://github.com/ninjasphere/go-ninja>`_

2. Make sure you are setup to cross compile go code, if you are getting the error:

.. code-block:: bash

    go build runtime: linux/arm must be bootstrapped using make.bash

you will need to do the following to enable cross compiling. You can skip this step if you just want to run your application locally.

.. code-block:: bash

    cd $GOROOT/src
    $ GOOS=linux GOARCH=arm ./make.bash --no-clean


3. Below are the basic files needed to run your application on the NinjaSphere

main.go

.. code-block:: go

	package main
	
	import (
		"github.com/ninjasphere/go-ninja/api"
		"github.com/ninjasphere/go-ninja/support"
	)

	var (
		info = ninja.LoadModuleInfo("./package.json")
	)
	
	// A Hello specifies a hello object.  It has a UUID that is a unique identifier
	// and a label which provides a human readable label.
	type Hello struct {
		UUID   string	`json:"uuid,omitempty"`
		Label  string	`json:"label,omitempty"`
	}

	// A World object is a collection of Hellos.
	type World struct {
		Version string  `json:"version,omitempty"`
		Hellos  []Hello `json:"hellos"`
	}

	//HelloApp describes the hello world application.
	type HelloApp struct {
		support.AppSupport
	}
	
	// Start is called after the ExportApp call is complete.
	func (a *HelloApp) Start(m *World) error {
		return nil
	}
	
	// Stop the hello module.
	func (a *HelloApp) Stop() error {
		return nil
	}
	
	func main() {
		app := &HelloApp{}
		err := app.Init(info)
		if err != nil {
			app.Log.Fatalf("failed to initialize app: %v", err)
		}
	
		err = app.Export(app)
		if err != nil {
			app.Log.Fatalf("failed to export app: %v", err)
		}
	
		support.WaitUntilSignal()
	}

package.json

.. code-block:: json

	{
	  "id": "unique.package.name",
	  "name": "hello-world",
	  "version": "0.0.1",
	  "description": "Ninja Sphere - Hello World",
	  "main": "hello-world",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "author": "First Last <email@email.com>",
	  "license": "MIT",
	  "topics": {},
	  "maxMemory": 10
	}


version.go

.. code-block:: go

	package main

	// Version describes the version number of this package.
	const Version = "0.0.1"


4. Compile

.. code-block:: bash

    $ go build


to cross compile so it will run on the sphere:

.. code-block:: bash

    $ GOARCH=arm GOOS=linux go build

5. Start monitoring MQTT, filtering out only messages with hello in them

.. code-block:: bash

    $ mosquitto_sub -h ninjasphere.local -t '#' | grep hello

5. In another terminal window, run on your local machine (replace --serial value with your sphere's serial number which can be found in the Nodes section under "Support Agent" in the web app http://ninjasphere.local/):

.. code-block:: bash

    $ DEBUG=t ./hello-world --mqtt.host=ninjasphere.local --mqtt.port=1883 --serial=ABCDEFGHIJKLM


you should see output similar to the following:

.. code-block:: bash

    {"params":[{"topic":"$node/serial/app/unique.package.name","schema":"http://schema.ninjablocks.com/service/app","supportedMethods":["setLogLevel","start","stop"],"supportedEvents":[],"id":"unique.package.name","name":"hello-world","version":"0.0.1","description":"Ninja Sphere - Hello World","author":"First Last \u003cemail@email.com\u003e","license":"MIT"}],"jsonrpc":"2.0","time":1434509228860}

Example Sphere Applications
===========================

	* `Sphere Go LED Controller <https://github.com/ninjasphere/sphere-go-led-controller>`_
