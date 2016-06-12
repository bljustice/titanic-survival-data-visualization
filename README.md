#Titanic Passenger Survival Status Based on Age

##Summary
For this project, I visualized the # of titanic passengers that surived or died based on their age. I decided to use a stacked bar chart to show each age groups subset of survival status. I used R to aggregate each survivor status using custom age groups I defined using R. The original dataset is names titanic_data.csv and the grouped version of this document used for the visualization is called titanic_grouped_data.csv.

##Design
As stated in the summary portion, I decided to use a stacked bar chart for the design of this project. Bar charts are very useful for visualizing categorical data so I thought this was the best choice for conveying the purpose of the visualization. I used grey and orange colors for the two different categories since they're two of my favorite colors with a legend to show users which color defined a particular subgroup. Finally, I used 2 different mouseover events to change the color of the particular bar to purple and display how many passengers were included in that particular group to make it easier to interpret the information displayed.

I recieved 3 different sets of feedback on the visualization. I have included screenshots as to how I changed my visualization based on them.

Below is a screenshot of my initial visualization draft.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-design.png)

To address the first set of feedback I received, I added a `transform` attribute to the text element appended by the mouseover function. This centered the text above each bar in the chart to make it easier to read. Below is the function with this attribute added and a screenshot of the visualization with the updates.
```javascript
function showSurvivalData(d, i) {
  svg.append("text")
    .attr({
      id: "text" + d.x + "-" + d.y + "-" + i,
      x: function() { return xScale(d.x); },
      y: function() { return yScale(d.y + d.y0); },
    })
    .attr("transform", "translate(40,-5)")
    .text(function() { return d.y; });
}
```
![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-feedback-implemented.png)

To address the second round of feedback I received, I updated the axes lines to be more complementary to the actual ticks for each axis. I did this using the following CSS, which can be seen in the `head` of the HTML file included in this repository. This CSS was used from one of Mike Bostock's stacked bar charts listed in the sources of this repository.
```CSS
.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}
```

I also added a y-axis label to make it more readable to users. Below is the final code used to render the y axis.
```javascript
svg.append("g")
  .attr("class", "y axis")
  .call(yAxis)
  .append("text")
   .attr("transform", "rotate(-90)")
   .attr("y", 6)
   .attr("dy", ".71em")
   .style("text-anchor", "end")
   .text("Number of Passengers");
   ```

Below is a screenshot of the results of these changes, which show much for user-friendly axes.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/second-feedback-implemented.png)

Finally, for my third set of feedback, I added 5 pixels of padding between each bar by updating the width values of each rectangle appended to the original svg layers I created. Below is the final code showing this update.

```javascript
layer.selectAll("rect")
  .data(function(d) { return d; })
  .enter().append("rect")
  .attr("x", function(d) { return xScale(d.x); })
  .attr("y", function(d) { return yScale(d.y + d.y0); })
  .attr("height", function(d) { return yScale(d.y0) - yScale(d.y + d.y0); })
  .attr("width", xScale.rangeBand() - 5)
  .on("mouseover", showSurvivalData)
  .on("mouseout", removeSurvivalData);
```
I also noticed that the mouseover and legend fonts were not the same as the axes, so I updated them to all match using this CSS.

```CSS
text {
  font-family: sans-serif;
  font-size: 11px;
}
```

Below is a screenshot of these changes in the final `index.html` file.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/third-feedback-implemented.png)


##Feedback

I reached out to 3 different people about my initial visualization and they each had different feedback, which I have listed below.

  1. The labels for each group is slightly hard to read when I hover over each bar in the graph. Maybe if they were centered it would be easier to read.
  2. The axes lines are very thick compared to the tick marks. Is there a way to make them more similar?
  3. There is no padding between each of the bars. I think it would be much easier on the eye if they had some padding between them.



##Resources
  * https://www.kaggle.com/c/titanic/data
  * https://bl.ocks.org/mbostock/1134768
  * http://bl.ocks.org/hlucasfranca/f133da4493553963e710
  * https://bl.ocks.org/mbostock/3886208
  * http://learnjsdata.com/group_data.html
