![FastLSH_LOGO](https://cloud.githubusercontent.com/assets/11495951/26155029/62aaf49a-3b44-11e7-8685-5f351001d4b9.png)  


# FastLSH-MPI
MPI (Message Passing Interface) version of FastLSH. It have been tested on Ubuntu 14.04, 16.04

## Requirement 
* openMPI >= 2.02 
    find installation guide [here](https://www.open-mpi.org/software/ompi/v2.1/)
* Boost >= 1.5  
`sudo apt-get install libboost-all-dev`
* You need to have MPI cluster ready and move the code to Network File System (NFS).  
    You can find configuration guide [here](http://mpitutorial.com/tutorials/running-an-mpi-cluster-within-a-lan/)
    
## Change Parameters 
`vim ./src/boostmpiCompute.cpp`  
change parameters between line 183-193

    size_t N = 1000; //# of vectors in the dataset
    size_t Q = 1000; //# of vertors in the queryset
    size_t D = 57; //# of dimensions
    size_t L = 200; //# of group hash
    size_t K = 1; //# the number of hash functions in each group hash
    double W = 1.2; //bucket width
    size_t  T = 100; // threshold
    int root_process = 0;
    char * inputPathSetN = "../testData/dataset1000NoIndex.csv" ;
    char * inputPathSetQ = "../testData/dataset1000NoIndex.csv";
    char * outputPath = "../testData/candidate.csv";
    
## Build
    cmake .  
    make
    
## Execution
Single Node:  
  `mpiexec -np 1 btl ^openib ./boostmpiLSH`  
Multi Nodes:  
  `mpiexec -np 4 -host <node1>, <node2>, <node3>, <node4>   -mca btl ^openib ./boostmpiLSH`

