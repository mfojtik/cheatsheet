* checking back on master nodes memory utilization
```
(
  (
    sum by (instance) (
      node_memory_MemTotal_bytes
      AND on (instance)
      label_replace( kube_node_role{role="master"}, "instance", "$1", "node", "(.+)" )
    ) - sum by (instance) (
      node_memory_MemFree_bytes
      + node_memory_Buffers_bytes
      + node_memory_Cached_bytes
      AND on (instance)
      label_replace( kube_node_role{role="master"}, "instance", "$1", "node", "(.+)" )
    )
  ) / sum by (instance) (
    node_memory_MemTotal_bytes
    AND on (instance)
    label_replace( kube_node_role{role="master"}, "instance", "$1", "node", "(.+)" )
  )
  * 100
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
