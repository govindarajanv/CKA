- Always start with $. followed by the topmost fieldname
- To get a particular field across all entries in a list, use [\*] 
- you can use index if you know the order
- To query a container name if order is going to be dynamic, use name like shown below
  - $.status.containerStatuses[?(@.name == 'container-name')].restartCount
- $ kubectl get pods -o=jsonpath='{ .items[0].spec.containers[0].image } {"\n"}{ .items[0].spec.containers[0].command }'
- for loop with json query path </br> '{ range .items[\*] </br> {.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"} </br> {end}'
 
