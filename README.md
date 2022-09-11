# PlanetaryComputerNotebooks

## Links

* https://planetarycomputer.microsoft.com/

## Usage types

### Use VS Code to connect to a remote Jupyter Kernel

See
* https://planetarycomputer.microsoft.com/docs/overview/ui-vscode/
* https://planetarycomputer.microsoft.com/docs/concepts/computing/

### Connect to the Gateway with local Jupyter instance running in Docker

See https://planetarycomputer.microsoft.com/docs/concepts/computing/

Same as in doc with `-v`

```
docker run -it --rm \
  -p 8888:8888 \
  -v "${PWD}":/home/jovyan/work \
  -e JUPYTERHUB_API_TOKEN=$JUPYTERHUB_API_TOKEN \
  -e DASK_GATEWAY__AUTH__TYPE="jupyterhub" \
  -e DASK_GATEWAY__CLUSTER__OPTIONS__IMAGE="mcr.microsoft.com/planetary-computer/python:latest" \
  -e DASK_GATEWAY__ADDRESS="https://pccompute.westeurope.cloudapp.azure.com/compute/services/dask-gateway" \
  -e DASK_GATEWAY__PROXY_ADDRESS="gateway://pccompute-dask.westeurope.cloudapp.azure.com:80" \
  mcr.microsoft.com/planetary-computer/python:latest \
  jupyter lab --no-browser --ip="0.0.0.0"
```

**Current problem**

Calling `.compute()` => `SystemError: unknown opcode. Probably beacuse there are python3.10 in the docker and 3.8 remote. See https://fugue-tutorials.readthedocs.io/tutorials/applications/debugging/unknown_opcode.html#:~:text=SystemError%3A%20unknown%20opcode%20The%20most%20common%20cause%20is,side%20before%20it%20is%20sent%20to%20the%20cluster.


The image we pull here is https://github.com/microsoft/planetary-computer-containers/blob/main/python/Dockerfile
and is based on 
`FROM pangeo/base-image:2022.07.27`


Dask-gateway compatibility is important https://github.com/pangeo-data/pangeo-docker-images#dask-gateway-compatibility

In PC JupyterHub: dask_gateway.__version__ = 0.9.0

