### Run the script to start cube

```sh
./docker-make.sh -U -I -i
```

### Upload a plugin to the chris store

To get the json representation of the plugin

```sh
docker run --rm fnndsc/pl-matrixmultiply matmultiply.py --json
```

### To register the plugin

Manipulate the make file or run the registration script under utils/scripts
