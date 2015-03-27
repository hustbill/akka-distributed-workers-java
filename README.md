
akka-distributed-workers-java
=======
Typesafe Activator template for distributed workers with Akka Cluster in Java.

[The Goal]
This example will explain how to implement distribtued workers with Akka cluster features. 
Because one single box obviously has limited resources, so our application need to distribtue work to many machines. 


The solution should support:

(1) elastic addition/removal of frontend nodes that receives work from clients
(2) elastic addition/removal of worker actors and worker nodes
(3) thousands of workers
(4) jobs should not be lost, and if a worker fails, the job should be retried

The design is based on Derek Wyatt's blog post Balancing Workload Across Nodes with Akka 2. This article describes the advantages of letting the workers pull work from the master instead of pushing work to the workers.

[Explore the Code - Master]
The heart of the solution is the Master actor that manages outstanding work and notifies registered workers when new work is available.

The Master actor is a singleton within the nodes with role "backend" in the cluster. This means that there will be one active master actor in the cluster. It runs on the oldest node.

You can see how the master singleton is started in the method startBackend in Main.java
