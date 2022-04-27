* checking back on nodes memory utilization
```
sum by (instance) (
  node_memory_MemTotal_bytes{job="node-exporter"}
  -
  node_memory_MemAvailable_bytes{job="node-exporter"}
)
```

* checking back on master nodes only:
```
sum by (instance) (
  1 - irate(
    node_cpu_seconds_total{mode="idle",instance=~"control-plane-.*"}[4m]
  )
  * on(namespace, pod) group_left(node) (
    node_namespace_pod:kube_pod_info:{node=~"control-plane-.*"}
  )
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
