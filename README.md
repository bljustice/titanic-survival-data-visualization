#Titanic Passenger Survival Status Based on Age

##Summary
For this project, I visualized the # of titanic passengers that surived or died based on their age. I decided to use a stacked bar chart to show each age groups subset of survival status. I used R to aggregate each survivor status using custom age groups I defined using R. The original dataset is names titanic_data.csv and the grouped version of this document used for the visualization is called titanic_grouped_data.csv.

##Design
As stated in the summary portion, I decided to use a stacked bar chart for the design of this project. Bar charts are very useful for visualizing categorical data so I thought this was the best choice for conveying the purpose of the visualization. I used grey and orange colors for the two different categories since they're two of my favorite colors with a legend to show users which color defined a particular subgroup. Finally, I used 2 different mouseover events to change the color of the particular bar to purple and display how many passengers were included in that particular group to make it easier to interpret the information displayed.

##Feedback
Below is a screenshot of the initial design of my bar chart.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-design.png")

I reached out to 3 different people about my initial visualization and they each had different feedback, which I have listed below.

  1. The labels for each group is slightly hard to read when I hover over each bar in the graph. Maybe if they were centered it would be easier to read.
  2. The axes lines are very thick compared to the tick marks. Is there a way to make them more similar?
  3. There is no padding between each of the bars. I think it would be much easier on the eye if they had some padding between them.



##Resources
https://www.kaggle.com/c/titanic/data
https://bl.ocks.org/mbostock/1134768
http://bl.ocks.org/hlucasfranca/f133da4493553963e710
