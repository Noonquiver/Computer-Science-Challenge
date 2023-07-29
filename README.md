# Computer-Science-Challenge
## LeetCode profile
https://leetcode.com/Noonquiver/
**Each exercise was solved using Swift programming language.**

### _Exercise #1: Merge Two 2D arrays by Summing Values_
https://leetcode.com/submissions/detail/1004703812/
<p>In order to solve this exercise I first summed the values with ids found on BOTH arrays and then added the values with ids only found in one array to finally sort the resulting array.</p>
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
                    nums1Copy = nums1Copy.filter{ $0[0] != id }
                    nums2Copy = nums2Copy.filter{ $0[0] != id }

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
### _Exercise #2: Path with Maximum Gold_
https://leetcode.com/submissions/detail/1004895810/
<p>I approached this exercise using Deep-First Search (DFS) algorithm which ends up exploring every possible path in order to find the solution.</p>
```swift
class Solution {
    func getMaximumGold(_ grid: [[Int]]) -> Int {
        var visitedCells = Set<[Int]>()
        var maximumGold = 0

        for row in 0 ..< grid.count {
            for column in 0 ..< grid[0].count {
                maximumGold = max(maximumGold, checkAdjacentCells(grid, location: [row, column], &visitedCells))
            }
        }

        return maximumGold
    }

    func checkAdjacentCells(_ grid: [[Int]], location: [Int], _ visitedCells: inout Set<[Int]>) -> Int {
        var gridColumns = grid[0].count
        var gridRows = grid.count
        var row = location[0]
        var column = location[1]
        var left = [row, column - 1]
        var right = [row, column + 1]
        var up = [row - 1, column]
        var down = [row + 1, column]
        var directions = [left, right, up, down]

        if row < 0 || row >= gridRows || column < 0 || column >= gridColumns || grid[row][column] == 0 || !visitedCells.insert(location).inserted {
            return 0
        }

        var maximumGold = 0
        var locationGold = grid[row][column]
        
        for nextLocation in directions {
            maximumGold = max(maximumGold, checkAdjacentCells(grid, location: nextLocation, &visitedCells))
        }

        visitedCells.remove(location)
        
        return maximumGold + locationGold
    }
}
```
