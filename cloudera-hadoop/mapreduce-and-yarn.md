# MapReduce and YARN

### MapReduce: Key Features

* Neither platform- nor language-specific
* Record-oriented data processing \(ket and value\)
* Facilitates task distribution across multiple nodes

### The Motivation for YARN

* MapReduce used all of the cluster's processing resources
* Now, multiple frameworks may exist on single cluster
* Each framework competes for compute and memory resources on the nodes
* Yarn was developed to manage this contention
  * Allocates resources to different frameworks based on demand, and on system administrator settings

