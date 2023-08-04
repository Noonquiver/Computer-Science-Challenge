# Computer-Science-Challenge
## LeetCode profile
https://leetcode.com/Noonquiver/
**Each exercise was solved using Swift programming language.**

### _Exercise #1: Merge Two 2D arrays by Summing Values_
https://leetcode.com/submissions/detail/1004703812/
<p>In order to solve this exercise I first summed the values with ids found on BOTH arrays and then added the values with ids only found in one array to finally sort the resulting array.</p>

#### _Time complexity_
<p>O(n * m * (n + m)) because the filtering done inside the inner loop is the dominant term, also note that the complexity is cubic and that O(n) is for the copy of **nums1** and O(m) is fot the copy of **nums2**.</p>

#### _Space complexity_
<p>O(n + m) because **mergedArray** holds elements from **nums1** and **nums2**.</p>

```swift
class Solution {
    func mergeArrays(_ nums1: [[Int]], _ nums2: [[Int]]) -> [[Int]] {
        var mergedArray = [[Int]]()
        var nums1Copy = nums1
        var nums2Copy = nums2

        for (index, array) in nums1.enumerated() {
            let id = array[0]
            let value = array[1]

            for (index2, array2) in nums2.enumerated() {
                let id2 = array2[0]
                let value2  = array2[1]

                if id == id2 {
                    let summedValue = value + value2
                    nums1Copy = nums1Copy.filter{ $0[0] != id } //nums1 items that have matching ids on nums2 array.
                    nums2Copy = nums2Copy.filter{ $0[0] != id } //nums2 items that have matching ids on nums1 array.

                    mergedArray.append([id, summedValue])
  
                    break
                }
            }
        }

        mergedArray = mergedArray + nums1Copy + nums2Copy

        return mergedArray.sorted{ $0[0] < $1[0] }
    }
}
```
### _Exercise #1 (Optimized): Merge Two 2D arrays by Summing Values_
https://leetcode.com/submissions/detail/1011748933/
<p>To optimize this exercise I converted each array in a dictionary, merged them into another and sorted it, successfully eliminating the nested loop issue.</p>

#### _Time complexity_
<p>O((n + m) log (n + m)) because sorting **mergedArray** is the dominant term.</p>

#### _Space complexity_
<p>O(n + m) because **mergedArray** holds elements from **nums1** and **nums2**.</p>

```swift
//Optimized solution

class Solution {
    func mergeArrays(_ nums1: [[Int]], _ nums2: [[Int]]) -> [[Int]] {
        
        // Initializing dictionaries to store key-value pairs from nums1 and nums2.
        var dictionary1 = [Int: Int]() 
        var dictionary2 = [Int: Int]()
        
        var mergedDictionary = [Int: Int]() // Dictionary that will store the merged key-value pairs.

        // Convert the 2D array nums1 into a dictionary.
        nums1.forEach { array in
            dictionary1[array[0]] = array[1]
        }

        // Convert the 2D array nums2 into a dictionary.
        nums2.forEach { array in
            dictionary2[array[0]] = array[1]
        }

        // Copy all pairs from dictionary1 into the mergedDictionary.
        dictionary1.forEach { (id, value) in
            mergedDictionary[id] = value
        }

        // For each pair in dictionary2, if the key is already present in mergedDictionary, sum up the values.
        // If not, just add the pair to mergedDictionary.
        dictionary2.forEach { (id, value) in
            if let existingValue = mergedDictionary[id] { // Optional unwrapping, existingValue will be null if there's no key equal to id.
                mergedDictionary[id] = existingValue + value
            } else {
                mergedDictionary[id] = value
            }
        }

        // Convert the merged dictionary back into a 2D array.
        let mergedArray = mergedDictionary.map { item in [item.key, item.value] }

        // Return the merged array sorted by keys.
        return mergedArray.sorted{ $0[0] < $1[0] }
    }
}
```
### _Exercise #2: Path with Maximum Gold_
https://leetcode.com/submissions/detail/1011747864/
<p>I approached this exercise using Deep-First Search (DFS) algorithm which ends up exploring every possible path in order to find the solution.</p>

#### _Time complexity_
<p>O(n^2 * m^2) because it would have to check each cell in the n * m size grid once.</p>

#### _Space complexity_
<p>O(n * m) because **visitedCells** holds at most n * m elements, the number of cells.</p>

```swift
class Solution {
    func getMaximumGold(_ grid: [[Int]]) -> Int {

        // A set to keep track of cells that have been visited.
        var visitedCells = Set<[Int]>()

        // Maximum gold that can be collected, initialized to 0.
        var maximumGold = 0

        // Loop over every cell in the grid.
        (0 ..< grid.count).forEach { row in
            (0 ..< grid[0].count).forEach { column in
                // For each cell, we try to get the maximum gold by checking its adjacent cells.
                maximumGold = max(maximumGold, checkAdjacentCells(grid, location: [row, column], &visitedCells))
            }

        }

        return maximumGold
    }

    /// Function to check the gold value of the adjacent cells.
    ///
    /// - Parameters:
    ///   - grid: The grid of cells containing gold values.
    ///   - location: Current location to check.
    ///   - visitedCells: Set containing the cells that have already been visited.
    /// - Returns: Maximum gold that can be collected from the current location.
    func checkAdjacentCells(_ grid: [[Int]], location: [Int], _ visitedCells: inout Set<[Int]>) -> Int {
        let gridColumns = grid[0].count
        let gridRows = grid.count
        let row = location[0]
        let column = location[1]
        
        // These arrays represent the possible moves from the current location.
        let left = [row, column - 1]
        let right = [row, column + 1]
        let up = [row - 1, column]
        let down = [row + 1, column]
        
        let directions = [left, right, up, down] // List of all possible moves.

        // If we're out of grid bounds or the current cell has a value of 0 or this cell has already been visited, return 0.
        if row < 0 || row >= gridRows || column < 0 || column >= gridColumns || grid[row][column] == 0 || !visitedCells.insert(location).inserted {
            return 0
        }

        var maximumGold = 0
        let locationGold = grid[row][column] // Amount of gold in the current cell
        
        // For each possible move, we recursively try to get the maximum gold.
        directions.forEach { nextLocation in
            maximumGold = max(maximumGold, checkAdjacentCells(grid, location: nextLocation, &visitedCells))
        }

        // Once we're done exploring from this cell, we mark it as unvisited for other paths.
        visitedCells.remove(location)
        
        // The gold collected from this path is the sum of gold in the current cell and the gold collected from its adjacent cells.
        return maximumGold + locationGold
    }
}
```
### _Exercise #3: Shortest Path Visiting All Nodes_

<p></p>

#### _Time complexity_
<p>O() because </p>

#### _Space complexity_
<p>O() because </p>

```swift
```
