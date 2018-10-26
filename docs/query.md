**Under Construction**

Academic article discussing CamFlow query features is available [online](http://camflow.org/publications/ccs-2018.pdf).

We assume a machine with CamFlow newly installed and running. 
More info on how to [install](https://github.com/CamFlow/documentation/blob/master/docs/installation.md) and [run](https://github.com/CamFlow/documentation/blob/master/docs/tutorial.md) CamFlow are available. 
To build and load the demo query, do as follow:
```
git clone https://github.com/camflow/libcamquery
cd libcamquery
make build
make load
```

You may need to install dependencies such as `elfutils-libelf-devel`.

To test that the query has been properly loaded:
```
dmesg
```

You should see the following entries:
```
Provenance: loading new query... (My Example Query)
Hello World!
Provenance: registering policy hook...
```

By default CamFlow does not generate provenance. We are going to change its configuration (more info [here](https://github.com/CamFlow/documentation/blob/master/docs/configuration.md)).
```
sudo camflow -a true
sudo camflow -a false
```

Those commands turned on and off whole-system provenance capture. You now run:
```
dmesg
```
You should see a list of edges and vertices.
