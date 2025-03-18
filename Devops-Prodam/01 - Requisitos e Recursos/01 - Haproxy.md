### HAPROYX

### Hardware recommendations 
The hardware requirements for HAProxy Enterprise depend on the workload it needs to manage:

- Item Only CPU and memory are taken into consideration.
- Item Disk size depends on your operating system and the volume of logs you want to keep.

##### Low-level workload

- Item TCP or HTTP traffic
- Item Up to 1000 conn/s
- Item Very low SSL traffic or gzip compression
<br>
- Item 1 CPU core
- Item 2 GB of RAM

##### Mid-level workload

- Item TCP or HTTP traffic (including HTTP manipulation)
- Item Up to 4000 conn/s
- Item Low SSL traffic or gzip compression
- Item 2 CPU cores
- Item 2 GB of RAM
- Item High-level workload
<br>
- Item TCP or HTTP traffic (including HTTP manipulation)
- Item Up to 20000 conn/s
- Item 10% of traffic ciphered (SSL) or compressed
- Item 2 CPU cores, as fast as possible
- Item 4G of RAM
- Item powerful network card
<br>

##### FONTE:
[Requisitos HAPROXY](https://www.haproxy.com/documentation/haproxy-enterprise/getting-started/installation/linux/)

