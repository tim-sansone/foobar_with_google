# foobar_with_google

Gearing Up for Destruction
==========================
As Commander Lambda's personal assistant, you've been assigned the task of configuring the LAMBCHOP doomsday device's axial orientation gears. It should be pretty simple -- just add gears to create the appropriate rotation ratio. But the problem is, due to the layout of the LAMBCHOP and the complicated system of beams and pipes supporting it, the pegs that will support the gears are fixed in place.

The LAMBCHOP's engineers have given you lists identifying the placement of groups of pegs along various support beams. You need to place a gear on each peg (otherwise the gears will collide with unoccupied pegs). The engineers have plenty of gears in all different sizes stocked up, so you can choose gears of any size, from a radius of 1 on up. Your goal is to build a system where the last gear rotates at twice the rate (in revolutions per minute, or rpm) of the first gear, no matter the direction. Each gear (except the last) touches and turns the gear on the next peg to the right.

Given a list of distinct positive integers named pegs representing the location of each peg along the support beam, write a function solution(pegs) which, if there is a solution, returns a list of two positive integers a and b representing the numerator and denominator of the first gear's radius in its simplest form in order to achieve the goal above, such that radius = a/b. The ratio a/b should be greater than or equal to 1. Not all support configurations will necessarily be capable of creating the proper rotation ratio, so if the task is impossible, the function solution(pegs) should return the list [-1, -1].

For example, if the pegs are placed at [4, 30, 50], then the first gear could have a radius of 12, the second gear could have a radius of 14, and the last one a radius of 6. Thus, the last gear would rotate twice as fast as the first one. In this case, pegs would be [4, 30, 50] and solution(pegs) should return [12, 1].

The list pegs will be given sorted in ascending order and will contain at least 2 and no more than 20 distinct positive integers, all between 1 and 10000 inclusive.

Solution
===================

```
def solution(pegs):
    peg_number = 2 * (sum(pegs[1::2]) - sum(pegs[::2])) + pegs[0]

    even = len(pegs) % 2 == 0

    if even:
        peg_number = 2 * (peg_number - pegs[len(pegs) - 1])
        
        if peg_number / 3 < 1:
            return [-1, -1]
        elif peg_number % 3 == 0:
            answer = [peg_number / 3, 1]
        else:
            answer =  [peg_number, 3]
    else:
        peg_number = 2 * (peg_number + pegs[len(pegs) - 1])

        if peg_number < 1:
            return [-1, -1]
        else:
            answer = [peg_number, 1]
            
    gear_rad = answer[0] / answer[1]
    for i in range(1, len(pegs)):
        gear_rad = pegs[i] - pegs[i-1] - gear_rad
        if gear_rad < 1:
            return [-1, -1]
        
    return answer
```
Ion Flux Relabeling
===================

Oh no! Commander Lambda's latest experiment to improve the efficiency of the LAMBCHOP doomsday device has backfired spectacularly. The Commander had been improving the structure of the ion flux converter tree, but something went terribly wrong and the flux chains exploded. Some of the ion flux converters survived the explosion intact, but others had their position labels blasted off. Commander Lambda is having her henchmen rebuild the ion flux converter tree by hand, but you think you can do it much more quickly -- quickly enough, perhaps, to earn a promotion!

Flux chains require perfect binary trees, so Lambda's design arranged the ion flux converters to form one. To label them, Lambda performed a post-order traversal of the tree of converters and labeled each converter with the order of that converter in the traversal, starting at 1. For example, a tree of 7 converters would look like the following:

   7
 3   6
1 2 4 5

Write a function solution(h, q) - where h is the height of the perfect tree of converters and q is a list of positive integers representing different flux converters - which returns a list of integers p where each element in p is the label of the converter that sits on top of the respective converter in q, or -1 if there is no such converter.  For example, solution(3, [1, 4, 7]) would return the converters above the converters at indexes 1, 4, and 7 in a perfect binary tree of height 3, which is [3, 6, -1].

The domain of the integer h is 1 <= h <= 30, where h = 1 represents a perfect binary tree containing only the root, h = 2 represents a perfect binary tree with the root and two leaf nodes, h = 3 represents a perfect binary tree with the root, two internal nodes and four leaf nodes (like the example above), and so forth.  The lists q and p contain at least one but no more than 10000 distinct integers, all of which will be between 1 and 2^h-1, inclusive.

Languages
=========

To provide a Java solution, edit Solution.java
To provide a Python solution, edit solution.py

Test cases
==========
Your code should pass the following test cases.
Note that it may also be run against hidden test cases not shown here.

-- Java cases --
Input:
Solution.solution(5, {19, 14, 28})
Output:
    21,15,29

Input:
Solution.solution(3, {7, 3, 5, 1})
Output:
    -1,7,6,3

-- Python cases --
Input:
solution.solution(3, [7, 3, 5, 1])
Output:
    -1,7,6,3

Input:
solution.solution(5, [19, 14, 28])
Output:
    21,15,29

Solution
==========
```
def check_node(node_val, h, check_val):
    if not check_val in range(1, node_val):
        return -1
    
    right = node_val - 1
    left = right - (2**(h-1) - 1)
    h -= 1

    if left == check_val or right == check_val:
        return node_val
    elif check_val > left:
        return check_node(right, h, check_val)
    elif check_val < left:
        return check_node(left, h, check_val) 
    
    
def solution(h, q):
    initial_val = 2**h - 1
    return [check_node(initial_val, h, val) for val in q]
```
