Project Proposal
-----------------
Dr. William Oropallo (Former Instructor I at the University of South Florida)

Author
-------
Mihir Patel   

Status
-------
Completed

Description
-------------   
This program is made for generating Huffman codes to compress a given string. A Huffman code uses a set of prefix code to compress the string with no loss of data (lossless). David Huffman developed this algorithm in the paper “A Method for the Construction of Minimum-Redundancy Codes,” [read it here.](http://compression.ru/download/articles/huff/huffman_1952_minimum-redundancy-codes.pdf) 

A program can generate Huffman codes from a string using the following steps:   
    
1) Generate a list of the frequency in which characters appear in the string using a map.     
2) Inserting the characters and their frequencies into a priority queue (sorted first by the lowest frequency and then lexicographically).       
3) Until there is one element left in the priority queue 
    - Remove two characters/frequencies pairs from the priority queue.        
    - Turn them into leaf nodes on a binary tree.     
    - Create an intermediate node to be their parent using the sum of the frequencies for those children.       
    - Put that intermediate node back in the priority queue.     
4) The last pair in the priority queue is the root node of the tree.    
5) Using this new tree, encode the characters in the string using a map with their prefix code by traversing the tree to find where the character’s leaf is. When traversal goes left, add a 0 to the code, when it goes right, add a 1 to the code.      
6) With this encoding, replace the characters in the string with their new variable-length prefix codes.   

In addition to the compress string, program also serialize the tree. Without the serialized version of the Huffman tree, currenyl the program can not decompress the Huffman codes. Tree serialization will organize the characters associated with the nodes using post order. During the post order when a node is visited,   

1) if it the node is a leaf (external node) then  add a L plus the character to the serialize tree string    
2) if it is a branch (internal node) then add a B to the serialize tree string    

For decompression, two input arguments will be needed. The Huffman Code that was generated by input compress method and the serialized tree string from input serializeTree method. The Huffman tree is built by deserializing the tree string by using the leaves and branches indicators. After the tree is back, decompress the Huffman Code by tracing the tree to figure out what variable length codes represent actual characters from the original string.   

So, for example, if we are compressing the string *“if a machine is expected to be infallible it cannot also be intelligent”:*   

| Character     | Frequency     | Prefix Code  |
| ------------- |:-------------:| ------------:|
| (space)       |     12        |    00        |
|   a           |     5         |   1011       |
|   b           |     3         |   10101      |
|   c           |     3         |   11000      |
|   d           |     1         |   010000     |
|   e           |     9         |   100        |
|   f           |     2         |   01011      |
|   g           |     1         |   010001     |
|   h           |     1         |   010001     |
|   i           |     8         |   011        |
|   l           |     6         |   1101       |
|   m           |     1         |   010011     |
|   n           |     6         |   1110       |
|   o           |     3         |   11001      |
|   p           |     1         |   010100     |
|   s           |     2         |   10100      |
|   t           |     6         |   1111       |
|   x           |     1         |   010101     |
    
*Our code would be:*   
011010110010110001001110111100001001001111101000001110100001000101010101001001 1000111110001000000111111001001010110000011111001011101111011101011101011101100 00011111100110001011111011101100111110010111101101001100100101011000001111101111 1001101110101101000110011101111    

*And our serialize tree would look like:*   
L LdLgBLhLmBBLpLxBLfBBLiBBLeLsLbBLaBBLcLoBLlBLnLtBBBB 
   
Furthermore, there are following abstractions-   
A) **std::string compress(const std::string inputStr)**    
  - Compress the input string using the method explained above.

B) **std::string serializeTree() const**    
  - Serialize the tree using the above method. We do not need the frequency values to rebuild the tree, just the characters on the leaves and where the branches are in the post order. 

C) **std::string decompress(const std::string inputCode, const std::string serializedTree)**   
  - Given a string created with the compress method and a serialized version of the tree, return the decompressed original string  

Please use test file HuffmanTreeTest.cpp to test your given input. 
      
Examples   
--------   
Below are some examples of how this code runs. The test file can also be used to get an idea of how the code can run. 

````````cpp
string test = "It is time to unmask the computing community as a Secret Society for the Creation and Preservation of Artificial Complexity”; 
/* 1000101011110000101001100110000010111011001110111101111100011001001011 0100100001111001110010011101101111010110010101010111110011000001110000 1011011110101100100010111110001100001111111111001011010011001011101001 1111101111001001110011110100111101111110000111001111111111010101110110 1001100111001001110110100110010011100101011000101100111100101001110000 0111010000000100111010100111001001000110010101100010110011110101110101 1110100010001000110001010110001111000001011001011101001101011001010101 
010010111101000111000011111111 */ 

string code = t.compress(test); 
/* LiLmLnBBLrLaBLtBBLPLdBLgLkBBLALIBLvLxBBBLhLlBLCLSBBBLsLpLfBBLoBBL LeLcLuLyBBBBBB */ 

string tree = t.serializeTree(); 
/* It is time to unmask the computing community as a Secret Society for the Creation and Preservation of Artificial Complexity */ 

string orig = t.decompress(code, tree);      
````````
