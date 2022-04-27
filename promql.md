* checking back on master nodes memory utilization
```
sum by (instance) (
  node_memory_MemTotal_bytes{job="node-exporter"}
  -
  node_memory_MemAvailable_bytes{job="node-exporter"}
)
```

* checking back on master nodes CPU
```
sum by (instance,mode) (
  rate(node_cpu_seconds_total[5m])
  AND on (instance)
  label_replace( kube_node_role{role="master"}, "instance", "$1", "node", "(.+)" )
)
```
