# R-Tree
R-tree based search developed with devide and conquer  recursive approach . It is almost 4 times faster than sequential scan.
	Input files and parameters (the setting of directory, or other parameters)
              
             Dataset file:- The 150k points in ‘Dataset.txt’.
             Queries file:- The 200 queries given in ‘quiries.txt’.
             Example file:- 10 data points and 1 query in ‘example.txt’
              









Image of Directory in my Local Machine
              

•	The datapoints and query provided in example text file is used to primarily test the code I developed . Also I gave it a try to visualize the RTree and RTree-based search.

•	The ‘Dataset’ text file and ‘quiries’ text file are input files for final execution of code for RTree and RTree-based search. 
                  
1.3	Other requirements
              
             Path of Input Files:-  String of path for input files to be copied.

Steps Required for Files Uploading, Execution in Colab
             
  
Step 1:-   Click the ‘Files’ icon and opening ‘sample data’ folder
            





Step 2:- After clicking upload , the input files can be selected from folder of local machine.

 




Step 3:- Path of the file can be copied from the menu that appears after clicking “:” beside the uploaded filename.

 

 







Program documentation



2.1 Program organisation

 There are two files named ‘RTree_Visualization.ipynb’ and  ‘RTree_MainToSubmit.ipynb’ .
These two files are independent of each-other.
•	In this ‘RTree_Visualization.ipynb’ file, I have defined the classes to build RTree and perform search of the queries and test it with the provided example in ILearn. 
•	The classes with same logic and functions are in the file ‘RTree_MainToSubmit.ipynb’ to take inputs from the dataset file and perform 200 range queries in queries file.

Brief Introduction of R-Tree:-
  An R-Tree is a spatial data structure used for indexing multi-dimensional data, particularly for efficient retrieval of spatial objects based on their spatial relationships. R-Trees can efficiently index objects with irregular shapes, as they use the concept of Minimum Bounding Rectangles (MBRs) to represent and organize objects in the tree.































     
Class Name	Description (detailed information)
	
Node	The class has the following attributes:

•	id: An identifier for the node.
•	child_nodes: A list containing child nodes of the current node. This is used for internal nodes.
•	data_points: A list containing data points associated with the current node. This is used for leaf nodes.
•	parent: A reference to the parent node.
•	MBR: A dictionary representing the Minimum Bounding Rectangle (MBR) of the node. The MBR is defined by the coordinates of its bottom-left corner (x1,y1) and the coordinates of its top-right corner (x2, y2).

There are several methods defined in the class:

•	perimeter(): Calculates and returns the half perimeter of the MBR of the node. The half perimeter is computed by summing the differences between the x and y coordinates of the MBR corners.
•	is_overflow(): Checks whether the node is overflowing, i.e., whether it has exceeded the upper bound (B) for either the number of data points (for leaf nodes) or the number of child nodes (for internal nodes).
•	is_root(): Checks whether the node is the root of the R-Tree by examining whether it has a parent. If it doesn't have a parent, it is considered the root.
•	is_leaf(): Checks whether the node is a leaf node by inspecting whether it has any child nodes. If it doesn't have any child nodes, it is considered a leaf node.



	
	
	
	
	
	


Class Name	Description (detailed information)
	
RTree	The RTree class represents an implementation of the R-tree data structure. R-tree is a spatial index structure used for efficient retrieval of spatial objects based on their spatial proximity. It organizes objects into a hierarchical structure of bounding rectangles, where each node represents a rectangle enclosing its child nodes or data points.

The RTree class initializes with an empty root node.

insert(self,u,p):-  
•	Parameter Description:- 
    u :-  Passed node
               p :- The data point
   self:- object of the class
•	Description:- The insert method is used to insert a data point p into the tree. It starts with the root node u. If the current node is a leaf node, it adds the data point to the node and checks for overflow. If overflow occurs, it invokes the handle_overflow method to handle it. If the current node is not a leaf, it selects a subtree v using the choose_subtree method and continues the insertion process recursively.

choose_subtree(self, u, p):-
               
•	Parameter Description:- 
   u :-  Passed node
   p :- The data point
   self:- object of the class
•	Description:- The choose_subtree method is used to select the best subtree for inserting a data point p into a non-leaf node u. It calculates the increase in perimeter for each child node and chooses the one with the minimum increase.

handle_overflow(self,u):- 

•	Parameter Description:- 
u :-  Passed node

•	Description:- The handle_overflow method is called when a node u overflows (exceeds its capacity). It splits the node into two new nodes u1 and u2 using the split method. If the overflow occurs in the root node, a new root node is created with u1 and u2 as its children. If the overflow occurs in a non-root node, `u` is deleted, and u1 and u2 become the new children of u's parent node w. If w also overflows, the method is called recursively to handle the overflow.

split(self,u):-
•	Parameter Description:- 
u :-  Passed node
•	Description:- The split method is responsible for splitting a node u into two new nodes s1 and s2. If u is a leaf node, it sorts the data points based on their X and Y coordinates and tries different divides to find the one with the minimum increase in perimeter. If u is an internal node, it sorts the child nodes based on their MBR’s coordinates and performs the same process to find the best divide. It returns the resulting s1 and s2 nodes.

update_mbr(self,node):- 
•	Parameter Description:- 
node:-  Passed node
•	Description:- The update_mbr method is used to update the MBR (Minimum Bounding Rectangle) of a node. If the node is a leaf, it collects the X and Y coordinates of its data points. If the node is an internal node, it collects the X and Y coordinates of its child nodes' MBRs. Then, it calculates the new MBR by finding the minimum and maximum values in each dimension and assigns it to the node's MBR.

peri_increase (self, node, p):- 
•	 Parameter Description:- 
    node:-  Passed node
    p:- data point
•	Description:- The peri_increase method is responsible for calculating the increase in perimeter after inserting a new data point p into a given node node. The perimeter increase represents the change in the boundary length of the Minimum Bounding Rectangle (MBR) of the node caused by the insertion of p. The calculation is performed by comparing the new perimeter with the original perimeter.


add_child (self, node, child):- 
•	 Parameter Description:- 
    node:-  Passed as a parent node of a child
    child:- Passed as a child node tobe added
•	Description:- The add_child method is responsible for adding a child node to a given parent node and updating the MBRs (Minimum Bounding Rectangles) accordingly. It is typically used in handling overflows during the R-tree insertion process.






 

add_data_point(self,node,data_point):-
•	 Parameter Description:- 
    node:-  Passed as node 
    data_point:- Input points 
•	Description:- The add_data_point method is responsible for adding a data point to a given node and updating the MBRs accordingly. It is used during the R-tree insertion process to add new data points to the appropriate leaf nodes





	
	
	
	
	



























2.2	Other Methods description:- 

              read_dataset():-  The read_dataset function reads a  text file containing data points, creates a dictionary for each data point, and stores them in a list. It then returns the list of data points as the result.
            read_queries():- The read_queries function reads a  text file containing queries, creates a dictionary for each query_id, x1,y1,x2,y2 , and stores them in a list. It then returns the list of queries as the result.
            search():- The search function is responsible for traversing the tree and finding the data points that fall within the specified query range.

•	First, it checks if the current node is a leaf node by calling the is_leaf() method. If it is a leaf node, it iterates over the data_points stored in that leaf node.For each data_point, it checks if the values of x and y fall within the specified query range. If the conditions are satisfied, it appends the data_point to the result list.
•	If the current node is not a leaf node, it means it is an internal node. In that case, the function iterates over each child node of the current node and checks if there is an overlap between the Minimum Bounding Rectangle (MBR) of the child node and the query range



overlap():- The overlap function takes in two parameters: mbr and query. These parameters represent the Minimum Bounding Rectangle (MBR) and the query range, respectively.
The purpose of the overlap function is to determine whether there is an overlap between the MBR and the query range. It does this by checking the positions of the edges of the rectangles.
•	The function starts by checking if the right edge of the query range is to the left of the left edge of the MBR, or if the left edge of the query range is to the right of the right edge of the MBR. If either of these conditions is true, it means that the two rectangles do not overlap horizontally, and the function returns False.
•	Next, the function checks if the bottom edge of the query range is above the top edge of the MBR, or if the top edge of the query range is below the bottom edge of the MBR. If either of these conditions is true, it means that the two rectangles do not overlap vertically, and the function returns False
•	If none of the above conditions are met, it means that there is an overlap between the two rectangles both horizontally and vertically. In this case, the function returns True to indicate that an overlap exists.
The overlap function checks if there is an overlap between a Minimum Bounding Rectangle (MBR) and a query range. It compares the positions of the edges of the two rectangles and returns True if there is an overlap, or False otherwise.





range_query():- 

It takes in two arguments: root represents the root node of a tree, and query is a dictionary containing the query parameters. The function starts by initializing an empty list called result. This list will store the data points that satisfy the query conditions.
•	The function uses the search function described above as inner function . This function takes in a node and a query as parameters. The search function is responsible for traversing the tree and finding the data points that fall within the specified query range.
•	After the search function has traversed the tree and populated the result list, the range_query function returns the length of the result list, which represents the number of data points that satisfy the query conditions.
In summary, the range_query function performs a range query on a tree structure. It starts from the root node and recursively traverses the tree, checking for overlaps between the query range and the nodes' data points or child nodes' MBRs. It collects the data points that satisfy the query conditions and returns the count of those data points.

build_rtree():-

The build_rtree function takes the  file containing data_points (dataset.txt) as a parameter. This dataset represents a collection of points that will be used to build an R-tree.The function starts by creating an instance of the RTree class and assigning it to the variable rtree. This will be the R-tree that  will be built.
•	The function determines the total number of points in the dataset by using the `len` function.

•	The function uses a progress bar to visualize the progress of building the R-tree. It creates a `tqdm` progress bar object and sets the total number of iterations to `total_points`. The `desc` parameter is used to provide a description for the progress bar, in this case, "Building R-tree".
•	The function then iterates over the dataset using a `for` loop. In each iteration, it retrieves the current `point` from the dataset. Inside the loop, it inserts the current `point` into the R-tree by calling the `insert` method of the `rtree` object, passing the root of the R-tree (`rtree.root`) and the current `point` as arguments.
•	After inserting the point, the function updates the progress bar by calling `pbar.update(1)`. This increments the progress bar by one unit, indicating that one point has been processed.
•	The function includes a `time.sleep(0.01)` statement. This line introduces a small delay of 0.01 seconds between each iteration. It is used to control the speed of the progress bar. 
•	Finally, after processing all the points in the dataset, the function returns the built R-tree (rtree).

In summary, the build_rtree function constructs an R-tree from a given dataset. It iterates over each point in the dataset, inserts it into the R-tree, and updates a progress bar to visualize the building progress. After processing all the points, it returns the constructed R-tree.

sequential_scan():- The sequential_scan  function performs a sequential scan of the dataset for a given set of queries. It reads the queries from a file, iterates over each query, iterates over each point in the dataset, and counts the number of points that satisfy the query conditions. It stores the query results in a list and returns it.


Analysing the Working of R-Tree

3.1	R-Tree establishment:- 

The R-tree is a balanced tree structure that recursively partitions the space into smaller     bounding rectangles. Each node in the R-tree represents a bounding rectangle that contains either other bounding rectangles or the actual objects in the database. The root of the tree represents the entire space, while the leaf nodes represent individual objects or groups of objects.

Handling Overflow and splitting in R-Tree data structure:-

When a node exceeds the maximum number of entries allowed, it needs to be split into multiple nodes. I have tried to sum up important points of handling overflow and splitting below:

1. Leaf Node Overflow Handling:
•	When a leaf node overflows,  two new leaf nodes are to be created out of it.
•	The entries of the overflowing node should be distributed among the two new nodes in a way that minimizes the overlap between the entries.
•	The bounding rectangles of the new nodes are to be adjusted to encapsulate their respective entries.
•	If necessary,  the changes are to be propagated to the parent node to accommodate the new nodes.

2. Internal Node Overflow Handling:
•	When an internal node overflows,  two new internal nodes are to be created out of it.
•	The child entries of the overflowing node should be distributed among the two new nodes in a way that minimizes the overlap between their bounding rectangles.
•	The bounding rectangles of the new nodes are to be adjusted to encapsulate their respective child entries.
•	If necessary,  the changes are to be popagated to the parent node(s) to accommodate the new nodes.

3. Choosing Split Axis and Entry Distribution:
•	The split axis is chosen based on a heuristic to minimize the area enlargement of the bounding rectangles.
•	The entry distribution aims to balance the number of entries between the resulting nodes.
•	Different algorithms can be used to determine the split axis and entry distribution, such as the quadratic split or the linear split.

4. Propagation of Changes:
•	When a node splits and new nodes are created, the changes must be propagated to the parent node(s).
•	If the parent node is not full,  the new nodes are to be inserted into the parent along with their adjusted bounding rectangles.
•	If the parent node is full and overflows,  the overflow must be handled recursively following the same split and distribution rules.
•	This propagation continues until it reaches a non-full parent node or the root node. If the root overflows, a new root is created.



For example we are taking:
Data Points:

x, y
1 3
4 1
2 5
5 3
7 2
8 4
3 6
0 7
10 4
8 1

Query

x1, y1, x2, y2
5 2 9 6

To construct an R-tree with the given data points and a maximum of 4 entries per node, and to demonstrate the overflow handling and splitting algorithm, let's step through the construction process.

Step 1: Initialization
An empty R-tree is created. This will be the root node of the tree.

Step 2: Inserting Data Points
The data points are to be inserted one by one into the R-tree using the insert algorithm. We start with the root node as the current node.

Insert (1, 3)
Since the root node is empty, we create a new leaf node and insert the point (1, 3) into it. The leaf node will look like this:


Leaf Node 1:
(1, 3)


Insert (4, 1)
We insert (4, 1) into the leaf node, which now becomes:

Leaf Node 1:
(1, 3), (4, 1)

Insert (2, 5)
We insert (2, 5) into the leaf node, which results in overflow. Therefore, we split the node.


Leaf Node 1:
(1, 3), (2, 5)

Leaf Node 2:
(4, 1)


Insert (5, 3)
We insert (5, 3) into the leaf node, which is now full. We split it again.

Leaf Node 1:
(1, 3), (2, 5)

Leaf Node 2:
(4, 1), (5, 3)


Insert (7, 2)
We insert (7, 2) into the leaf node, which results in overflow. We split it.


Leaf Node 1:
(1, 3), (2, 5)

Leaf Node 2:
(4, 1), (5, 3), (7, 2)










Insert (8, 4)
We insert (8, 4) into the leaf node, which is now full. We split it.


Leaf Node 1:
(1, 3), (2, 5), (4, 1)

Leaf Node 2:
(5, 3), (7, 2), (8, 4)


Insert (3, 6)
We insert (3, 6) into the leaf node, which results in overflow. Split it.


Leaf Node 1:
(1, 3), (2, 5), (3, 6)

Leaf Node 2:
(4, 1), (5, 3), (7, 2), (8, 4)


Insert (0, 7)
We insert (0, 7) into the leaf node, which is now full. Split it.


Leaf Node 1:
(0, 7), (1, 3), (2, 5), (3, 6)

Leaf Node 2:
(4, 1), (5, 3), (7, 2), (8, 4)


Insert (10, 4)
We insert (10, 4) into the leaf node, which results in overflow. Split it.


Leaf Node 1:
(0, 7), (1, 3), (2, 5), (3, 6)

Leaf Node 2:
(4, 1), (5, 3), (7, 2), (8, 4)

Leaf Node 3:
(10, 4)





Step 3 : Constructing Internal Nodes

We have reached a point where we need to construct internal nodes to create a hierarchical structure in the R-tree.


Leaf Node 1:
(0, 7), (1, 3), (2, 5), (3, 6)

Leaf Node 2:
(4, 1), (5, 3), (7, 2), (8, 4)

Leaf Node 3:
(10, 4)

To construct the internal nodes, we treat each leaf node as a single entry and iteratively group them into parent nodes until we reach the root.


Level 1:
Internal Node 1:
    Leaf Node 1
    Leaf Node 2

Internal Node 2:
    Leaf Node 3




Level Root:
Root Node:
    Internal Node 1
    Internal Node 2


Step 4: Handling Query
Now let's perform a query to search for points within the given range (x1, y1, x2, y2) = (5, 2, 9, 6).

Starting from the root node, we evaluate the query against each child node's bounding rectangle. If the bounding rectangle overlaps with the query range, we descend into that child node and repeat the process until we reach the leaf nodes.







In this case, the root node's child nodes are both internal nodes. We evaluate the query against their bounding rectangles.


Root Node:
    Internal Node 1
    Internal Node 2

Internal Node 1's bounding rectangle does not overlap with the query range, so we do not descend into it. However, Internal Node 2's bounding rectangle overlaps with the query range, so we descend into it.


Internal Node 2:
    Leaf Node 3


Leaf Node 3's bounding rectangle also overlaps with the query range. Therefore, we examine the individual points within Leaf Node 3.


Leaf Node 3:
(10, 4)


The point (10, 4) lies within the query range, so it is considered a result of the query.

Overall, the R-tree structure allows us to efficiently prune irrelevant branches during the query process and retrieve the points that satisfy the given query range.

I have tried to visualize the R-Tree with directed graph approach. The plan is to define an  inner function  to recursively traverse the R-tree and add nodes and edges to the graph G. If the current node is a leaf node, it is added to the graph as a yellow-colored leaf node with the label "Leaf Node." If the current node is an internal node, it is added to the graph as a pink-colored internal node with the label "Internal Node." The function then recursively traverses the child nodes of the current node.

Library usage:- 
Matplotlib
Networkx










Code Screenshot:-

 

 












Visualization of R-Tree

 




























Implementing R-Tree Based Search with this example And Visualizing:


 

Here the data points found by the query are marked with red  colorand all RTree nodes points are marked with blue color

















Sequential-based search and R-Tree based search:-

Sequential-based search and RTree-based search are two different approaches for searching spatial data. Their differences are explored through the project.

1.	Sequential-based Search:

Sequential-based search, also known as linear search, involves scanning the entire dataset sequentially to find the desired objects. In the context of spatial data, this means examining each object one by one and checking if it lies within a specified region.

Function for sequential scan:-

 

Parameter details:- 

dataset:-It refers to a list of dictionary containing id , x co-ordinates and y co-ordinates of each data point present in dataset text file.

query_file:-It refers to a text file containing all 200 queries.

total_queries:- Number of all queries present in query file

Approach:- 

The function searches for data points which belong to the query region for each query and counts total number of data points belong to each query bound. Also performs a progress bar of sequential scan with package ‘tqdm’ and description labelled as ‘Sequential Scan’. 
It returns a list of tuples . Each tuple contains id of each query and number of points returned by that query.






Screenshot of Executing Sequential Scan and Building RTree:-

 



Result Screenshot:- 

            

Advantages:
•	Simplicity: Sequential search is straightforward to implement since it involves iterating over the dataset.
•	Suitable for small datasets: When the dataset is small, sequential search can be performed quickly enough.

Disadvantages:
•	Inefficiency for large datasets: With large datasets, the time complexity of sequential search becomes a significant drawback. It has a linear time complexity of O(n), where n is the number of objects in the dataset. As a result, the search time increases linearly with the dataset size.



2.	RTree-based Search:

RTree-based search utilizes the R-tree data structure, which is specifically designed for indexing spatial data. R-trees organize objects hierarchically into a tree structure, enabling efficient spatial queries by reducing the search space.

The Divide-and-Conquer Approach 
Basically search function is defined to perform a range query by traversing the spatial index. It takes two parameters: node, representing the current node being examined, and query, representing the range query specified by the user. It goes with divide and conquer approach with two tasks:- 
•	Leaf Node
•	Internal Node
Task Description in Brief:-
•	If the current node is a leaf node , the function iterates through each data point within the node. It checks if the data point falls within the specified query range using the query dictionary. If it does, the data point is appended to the result list .
•	If the current node is not a leaf node , the function iterates through each child node. It checks if the child node overlaps with the query range by using the overlap function.

                   If there is an overlap, the function recursively calls itself on the child node to further explore its subtree. 
                   If the child node does not overlap, it ends recursion with that child node to avoid unnecessary recursion calls .
                    If  there is no overlap between a child node's minimum bounding rectangle (MBR) and the query range, it means that the subtree rooted at that child node does not contain any data points that can possibly satisfy the query. In such cases, there is no need to continue exploring that subtree further.























Diagarm:- 

 


I tried to clearly explain the divide and conquer approach applied in search  with this flowchart.




















Code Snippet:- 

 

 


Processing for 200 Quiries Screenshot:-

 


Result Screenshot:- 

   

The whole result is written in ‘output.txt’ file

Advantages:
•	Efficient search: R-trees enable faster searches by exploiting the spatial relationships among objects. Instead of examining every object, the search is guided by traversing the tree structure and discarding irrelevant branches.
•	Spatial indexing: R-trees provide spatial indexing capabilities, which means that objects are organized based on their spatial proximity. This allows for efficient  finding objects within a specified region) and nearest neighbour searches.
•	Scalability: R-trees can handle large datasets efficiently, as their performance does not degrade linearly with the dataset size. The time complexity for searching an R-tree is typically sublinear or logarithmic, depending on the specific implementation.

Disadvantages:
•	Complexity: Implementing and maintaining an R-tree can be more complex than sequential search due to the need for tree construction and maintenance algorithms.
•	Overhead: R-trees incur additional storage overhead since they require maintaining the tree structure and associated metadata.









Executing The Program and Result:-

Building R-Tree and Sequential Scan:-

 

R-Tree Based Search

 

Result of Output file and File writing Code snippet

 


According to the output the developed R-Tree based search is 4 times faster than sequential scan.
 

 
       

References and Gratitude:-

Important Libraries referenced:-
https://github.com/networkx/networkx
https://matplotlib.org/stable/index.html

Important Resources referenced:-
https://www.w3schools.com/
https://ilearn.mq.edu.au/mod/url/view.php?id=7383247
https://www.programiz.com/dsa/divide-and-conquer






