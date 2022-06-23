## 630. Course Schedule III (23/06/2022)

### Problem:

There are  `n`  different online courses numbered from  `1`  to  `n`. You are given an array  `courses`  where  `courses[i] = [durationi, lastDayi]`  indicate that the  `ith`  course should be taken  **continuously**  for  `durationi`  days and must be finished before or on  `lastDayi`.

You will start on the  `1st`  day and you cannot take two or more courses simultaneously.

Return  _the maximum number of courses that you can take_.

**Example 1:**

**Input:** courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]

**Output:** 3
Explanation: 
There are totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day. 
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day. 
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.

**Example 2:**

**Input:** courses = [[1,2]]

**Output:** 1

**Example 3:**

**Input:** courses = [[3,2],[4,3]]
**Output:** 0

**Constraints:**

-   `1 <= courses.length <= 104`
-   `1 <= durationi, lastDayi  <= 104`

### Solution:

#### Using Dynamic Programming (Time limit exceeded)
**Algorithm**
> Use `knapsack algorithm` to figure out the maximum reachable course duration by taking possiblity of **taking** or **not taking** the course

- Sort list of courses based on `lastDay`.
- Make recursive call to `scheduleCourseRec` with two condition
  - _When current course is taken _ - add current course time to `time` and increment index
  - _When current couse is not taken_ - increment `index`
- Find max of both recursive calls and return it.
- Use `dp` matrix to store and reuse the state at each combination of `courses` length and max duration of the last day.

```Java
class Solution {
    public int scheduleCourse(int[][] courses) {
        int len = courses.length;
        
        List<Course> list = new ArrayList<>();
        for(int[] course: courses) {
            list.add(new Course(course[0], course[1]));
        }
        Collections.sort(list, (a, b) -> a.lastDay - b.lastDay);
        
        int maxTime = list.get(len - 1).lastDay;
        Integer[][] dp = new Integer[len][maxTime + 1];
        
        return scheduleCourseRec(list, 0, 0, dp);   
    }
    
    private int scheduleCourseRec(List<Course> courses, int index, int time, Integer[][] dp) {
        if(index == courses.size()) return 0;
        if(dp[index][time] != null) return dp[index][time];
        
        Course course = courses.get(index);
        dp[index][time] = scheduleCourseRec(courses, index + 1, time, dp);
        
        if(time + course.duration <= course.lastDay) {
            dp[index][time] = Math.max(dp[index][time], 
                            1 + scheduleCourseRec(courses, index+1, time + course.duration, dp));
        }
        return dp[index][time];
    }
}

class Course {
    int duration;
    int lastDay;
    
    public Course(int duration, int lastDay) {
        this.duration = duration;
        this.lastDay = lastDay;
    }
}
```

**Time Complexity: ** `O(n∗d)` where n is size of courses array and d is maximum duration of last day. We are using `dp` matrix with dimension `n*d` so this is the max states our algorithm will go through.

**Space Complexity: ** `O(n∗d)` to store 2d matrix.

#### Using greedy approach

**Algorithm**
- Sort list of courses based on `lastDay`.
- Create a max-heap (`queue`) to store duration of course. Create `time` to store time taken to complete courses till now.
- Iterate through course list and for each course:
  - If the course can be completed within `lastDay` of course then add course `duration` into the queue and add duration to `time`.
  - Else, find the maximum duration course encountered till now. Remove that course to accomodate current course. If `queue` contains the course with `duration` more than current course duration then replace that in queue and update `time`.
- At the end of iteration, `queue` contains duration of courses that can be completed. Return size of `queue`.


```Java
class Solution {
    public int scheduleCourse(int[][] courses) {
        int len = courses.length;
        List<Course> list = new ArrayList<>();
        for(int[] course: courses) {
            list.add(new Course(course[0], course[1]));
        }
        Collections.sort(list, (a, b) -> a.lastDay - b.lastDay);
        
        int time = 0;
        PriorityQueue<Integer> queue = new PriorityQueue<>((a, b) -> b - a);
        
        for(Course course: list) {
            if(time + course.duration <= course.lastDay) {
                // pick current course
                queue.offer(course.duration);
                time += course.duration;
            }else{
                if(!queue.isEmpty() && queue.peek() > course.duration) {
                    // drop biggest course duration encountered till now
                    // pick current course
                    time -= queue.poll();
                    time += course.duration;
                    queue.offer(course.duration);
                }
            }
        }
        // queue contains duration of courses to be picked
        return queue.size();
    }
}

class Course {
    int duration;
    int lastDay;
    
    public Course(int duration, int lastDay) {
        this.duration = duration;
        this.lastDay = lastDay;
    }
}
```

**Time Complexity: ** `O(n log n)` for inserting n elements into priority queue.

**Space Complexity: ** `O(n)` to store n elements in priority queue.