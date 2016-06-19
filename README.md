#Titanic Passenger Survival Status Based on Passenger Class

##Summary
For this project, I visualized the number of titanic passengers that survived or died based on their passenger class, which is supposed to resemble their socio-economic class. I decided to use a stacked bar chart to show this. I used R to aggregate each survivor's status using each unique passenger class included in the dataset, and wrote a new csv with this aggregated data. The original dataset is names `titanic_data.csv` and the grouped version of this document used for the visualization is called `titanic_grouped_data.csv`.

Class 1 is the highest class and class 3 is the lowest class. Based on my analysis I found that as passenger class lowered, the amount of people in that class that died increased, especially in the lowest class. This visualization also shows how much larger the lowest class of passengers is compared to the other two classes (about 55% of all passengers are in the 3rd class).

##Design
As stated in the summary portion, I decided to use a stacked bar chart for the design of this project. Bar charts are very useful for visualizing categorical data so I thought this was the best choice for conveying this visualization's story. I used grey and orange colors for the two different categories since they're two of my favorite colors, and a legend to show users which color defines a particular subgroup. Finally, I used 2 different event handlers to add interactivity. The first changes the color of the particular bar hovered over to purple and displays how many passengers are included in that particular class. It also shows what percentage that group makes up of all of the passengers combined. I feel this percentage helps users not only compare the size of each passenger group to others and to the total number of passengers overall, but to also show how survival rate decreases as class decreases. The second event simply removes the element created from the first event once a user navigates away from that part of the chart, and returns the element to its original state.

Below is a screenshot of my initial visualization draft.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-design.png)

I received 3 different sets of feedback on the visualization. I have included screenshots as to how I changed my visualization based on them. To address the first set of feedback I received, I subtracted 100 pixels from the `width` attribute of my rectangle layer, which helped shrink the bars in the visualization and center them over their tick marks. Below is the updated `layer` element I used to do this.

```javascript
layer.selectAll("rect")
  .data(function(d) { return d; })
  .enter().append("rect")
  .attr("x", function(d) { return xScale(d.x) + 50; })
  .attr("y", function(d) { return yScale(d.y + d.y0); })
  .attr("height", function(d) { return yScale(d.y0) - yScale(d.y + d.y0); })
  .attr("width", xScale.rangeBand() - 100)
  .on("mouseover", showSurvivalData)
  .on("mouseout", removeSurvivalData);
```
Below is a screenshot of my visualization after this update.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-feedback-implemented.png)

To address the second round of feedback, I increased the right side padding from 50 pixels to 75 pixels to avoid cutting off the right side of the legend. Below is the updated object I used to store the margins.

```javascript
var margin = {top: 20, right: 75, bottom: 35, left: 40}
```

Below is a screenshot of my visualization, which shows the full legend being displayed.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/second-feedback-implemented.png)

Finally, for my third set of feedback, I updated my mouseover function to create a `transform` attribute that was based on the particular `d.y` value being passed to it. The reason I did this is because numbers over 100 take up more space than numbers below 100 and thus didn't center equally. To account for this, I used a conditional function to return 1 of 2 possible `transform` attributes based on the input value from `d.y`.

Below is the final `showSurvivalData` mouseover function that I used to do this.  

```javascript
function showSurvivalData(d, i) {
  svg.append("text")
    .attr({
      id: "text" + d.x + "-" + d.y + "-" + i,
      x: function() { return xScale(d.x); },
      y: function() { return yScale(d.y + d.y0); },
    })
    .attr("transform", function() {
      if (d.y >= 100) {
        return "translate(63,-5)";
      } else {
        return "translate(68,-5)";
      }
    })
    .text(function() { return [d.y, ((d.y / totalPassengers) * 100).toFixed(2) + "% of Total Passengers"]; });
}
```

Below is a screenshot of these changes in the final `index.html` file.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/third-feedback-implemented.png)

##Feedback
I reached out to 3 different people about my initial visualization and they each had different feedback, which I have listed below.

  1. The bars in the visualization are too wide and don't seem centered above the x axis tick marks. Is there a way to center them and shrink them?
  2. The legend appears to be slightly cut off on the right side. Is there a way to show the full legend?
  3. Both data points that are shown by the mouseover events aren't centered. Is there a way to center these to make it more legible?

##Resources
  * https://www.kaggle.com/c/titanic/data
  * https://bl.ocks.org/mbostock/1134768
  * http://bl.ocks.org/hlucasfranca/f133da4493553963e710
  * https://bl.ocks.org/mbostock/3886208
  * http://learnjsdata.com/group_data.html
