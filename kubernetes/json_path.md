# JSON Path

## Query examples

$.car.wheels[0].model  (1ast item)  
$.car.wheels[*].model  (all items)  

$.prizes[?(@.year == 2014)].laureates[*].firstname  (prizes, year 2014)  
$.prizes[?(@.year == 2014)].laureates[*].firstname  

$.status.containerStatuses[?(@.name == 'redis-container')].restartCount  

$[0]  first item  
$[0,3]  first and fourth  
$[0:4]  first to fourth (including fourth)  
$[0:4:2]  every second  
$[-1:0]  last item  
$[-1:]  same  

## Kubectl json path usage

1, Run simple command  
- kubectl get pods
- kubectl get nodes
	
2, Get info in json format
- kubectl get pods -o json
	
3, prepare json path query
- .items[0].spec.containers[0].image
	
4, combine the two
- kubectl get pods -o=jsonpath '{ .items[0].spec.containers[0].image }'
		
- kubectl get nodes -o=custom-columns="column name":"json path"
- kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU: .status.capacity.cpu
	
- kubectl get nodes --sort-by= .status.capacity.cpu
	
- kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
	
- kubectl get persistentvolumes --sort-by='{.spec.capacity.storage}'