# Kube Node Tracer

Troubleshooting transient/ intermittent networking issues is a tedious process, oftentimes requiring packet trace collection and analysis to determine the root-cause. The container orchestration environment Kubernetes (K8s) poses another set of challenges due to its potential scale, ephemeral nature, and layers of abstraction. 

The `Kube Node Tracer` aims to provide K8s users a tool to perform rolling packet captures on the host network namespace for nodes within a cluster for use in troubleshooting transient networking issues. To limit the impact on cluster resources, `Kube Node Tracer` offloads packet capture files to a sink (ex. Google Cloud Storage) for later analysis. 

## Usage
This tool utilizes `tcpdump` to perform rotating packet captures on the node's primary interface thus the normal flags can be tuned based on requirements:

- -s - Snap length      (Default: 96)
- -B - Buffer Size      (Default: 1000)         
- -C - File Size        (Default: 100)           
- -W - File Count       (Default: 100)                     

The following `tcpdump` flags are optional:

- -i - Interface        
- -f - Filter           (Note: This is a wrapper around tcpdump filter)
- -G - Rotate Seconds  

`Kube Node Tracer` requires Google Cloud Storage (GCS) bucket to act as a sink for the packet traces thus the following flag is required:
- -b - GCS Bucket Name 

_Note: If using GKE the GCE VMs must have write permissions (scope) to the GCS bucket._

Lastly if running the `Kube Node Tracer` as a stand-alone container (ex. Docker) then the node name will need to be specified:
- -N - Node name        (Note: This is automatically generated by metadata specs in DaemonSet when using K8)

## Deployment

To build `Kube Node Tracer` simply run the following `Docker` command to build and image using the Dockerfile: 
```
docker build -t gke-node-tracer . 	
```

`Kube Node Tracer` should be deployed as priviled DaemonSet which operates in the host network namespace. See /examples for sample DaemonSet.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for details.

## License

Apache 2.0; see [`LICENSE`](LICENSE) for details.

## Disclaimer

This project is not an official Google project. It is not supported by
Google and Google specifically disclaims all warranties as to its quality,
merchantability, or fitness for a particular purpose.
