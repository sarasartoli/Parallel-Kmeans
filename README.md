##Implementation of the K-means Clustering Algorithm Procedural and Parrallel

### 1. Problem Description 

Clustering is the task of assigning a set of objects into groups (called clusters) so that the objects in the same cluster are more similar (in some sense or another) to each other than to those in other clusters. k-means clustering is a method of clustering which aims to partition n data points into k clusters (n >> k) in which each observation belongs to the cluster with the nearest mean.  The nearness is calculated by Euclidian distance function. We implemented diffrent variations of k-means clustering algorithm. More specifically, we implemented the procedural k-means in C, the parallel kmeans with both MPI and open MP.

###2. Basic  k-means algorithm - Procedural

The folowing is the pseudocode for the basic K-measn Algorithm:
```
Input: number of clusters K, temination limit L , data points $x_1,...,x_n
Output: A map from points to clusters 1,...,K

Place centroid c_1,…,c_k at the first k data points
Repeat until the maximum of disatance between cluster centers is less than L
	For each point x_i
		Find nearest centroid c_j
		Assign point x_i to cluster j
		Let new centroid c_j be mean of all points x_i assigned to cluster j in the previous step

```
<!--![Test 1 Result 2](https://github.com/maederayati/Parallel-Kmeans/blob/master/Test1_result2.jpg) <br><br>-->


###3. Basic  k-means algorithm - Parallel
```
Input: number of clusters K, temination limit L , data points x1,...,x_n
Output: A map from points to clusters 1,...,K

Start MPI environment 
	Set  number of processors as mySize
	Set  the ID of processors as myRank
	Set number of threads as NUM_THREADS	
	Set Portion as arraysize/mySize
	Initialize the test data
	Initialize the first centroids to be the first k data points	
	Set Diff to a large number
		while L<Diff
			Start open mp environment
        			Set  the ID of threads as ID
        			Set threadPortion as portion/NUM_THREADS
       				Each thread, computes cluster number for threadPortion size of data 
			Close open mp environment
			each processor computes the summation of data points for each diminetion and cluster and broadcast 
			each processor computes the number of data points in each cluster and broadcast 
			processor rank 0 computes the new centroids based on the summations and counts recieved 
			processor rank 0 updates Diff and broadcast 		
Close MPI environment

```

###4.Improvements

The first improvement that we made to the basic algorithm was to choose the initial centroids. Instead of choosing the centroids random we pick k data point from the data set which are as far as possible. The algorithm for finding distant data points is as follows.

```
Input: number of clusters K, data points x1,...,x_n
Output: K cluster centers, c_1,...,c_k

Let c_1 be x_1
Let c_2 be the data point which is farthest from c_1
let t=3
while t<=k
	For any data point x_i, 
		Find the minimum of distance to current clusters
	Let j be the index of the data point whose mimimum distance is maximum. 
	let c_t be x_j
	t++
```


