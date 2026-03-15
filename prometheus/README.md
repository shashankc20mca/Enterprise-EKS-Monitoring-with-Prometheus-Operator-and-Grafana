In order to use the prometheus as node_discovery we need to grant the ec2_describe role to the instance on which prometheus container is running only then it 
be able to discovery the worker node endpoints on the eks cluster as they scale up and down
And also ensure that you have open the node exporter default port that is 9100 on worker node sg for respective instance to reach on that port to scrape metric 
Provided that node exporter installed on all worker nodes as daemonset


