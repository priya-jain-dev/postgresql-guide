### **What is `Window Functions`?**
Window Functions compute their result based on a sliding window frame, a set of rows that are somehow related to the current row.
![](https://github.com/priya-jain-dev/postgresql-guide/blob/master/window-functions-guide/window-function.png) 

### **Aggregate Functions vs. Window Functions**
Easiest way to understand window function is to start by reviewing the aggregate function (SUM, AVG, MIN...). Aggregate fun. aggregate data 
from a set of rows into a single row. In Window function, it does not reduce the no. of rows
return.
Aggregate Functions            |  Window Functions
:-------------------------:|:-------------------------:
![](https://github.com/priya-jain-dev/postgresql-guide/blob/master/window-functions-guide/aggregate-function.png)  |  ![](https://github.com/priya-jain-dev/postgresql-guide/blob/master/window-functions-guide/window-function-work.png)