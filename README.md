#Titanic Passenger Survival Status Based on Passenger Class

##Summary
For this project, I visualized the number of titanic passengers that survived or died based on their age group, gender, and passenger class. I decided to use a stacked bar chart to show these. I used R to aggregate each passenger's survival status based on each of the characteristics mentioned above, and wrote a new CSV file for each characteristic. The original dataset is names `titanic_data.csv` and the grouped versions have been provided below.

  * `grouped_by_age_group.csv`
  * `grouped_by_gender.csv`
  * `grouped_by_passenger_group.csv`

Three apparent trends can be seen from the visualizations, which have been listed below.

  1. The majority of Titanic passengers did not survive the crash.
  2. Females had a higher survival rate than males. This is most likely caused by the fact that women and children were unloaded off the Titanic before men.
  3. The upper class (class 1) had a higher survival rate than the lower class (class 3). This was most likely caused by the fact that the upper class was unloaded before the other 2 classes, and that the lower class didn't even have life boats to transport them to safety.     

##Design
As stated in the summary portion, I decided to use a stacked bar chart for the design of this project. Bar charts are very useful for visualizing categorical data so I thought this was the best choice for conveying this visualization's story. I used grey and orange colors for the two different categories since they're two of my favorite colors, and a legend to show users which color defines a particular subgroup. Finally, I used 2 different event handlers to add interactivity. The first changes the color of the particular bar hovered over to purple and displays how many passengers are included in a particular category. It also shows what percentage that group makes up of all of the passengers combined. I feel this percentage helps users not only compare the size of each group to others and to the total number of passengers overall, but to also more easily show the trends mentioned in the summary and conclusion portions of the final `index.html` file. The second event simply removes the element created from the first event once a user navigates away from that part of the chart, and returns the element to its original state.

Below is a screenshot of my initial visualization draft.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/first-design.png)

I received 4 different sets of feedback on the visualization. I have included screenshots as to how I changed my visualization based on them. Originally I only showed passenger class and how that affected survival status. However based on my fourth round of feedback, I added to additional variables to show how they also affected survival status. The first three rounds of feedback only reflect the single variable visualization.

To address the first set of feedback I received, I subtracted 100 pixels from the `width` attribute of my rectangle layer, which helped shrink the bars in the visualization and center them over their tick marks. Below is the updated `layer` element I used to do this.

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

For my third set of feedback, I updated my mouseover function to create a `transform` attribute that was based on the particular `d.y` value being passed to it. The reason I did this is because numbers over 100 take up more space than numbers below 100 and thus didn't center equally. To account for this, I used a conditional function to return 1 of 2 possible `transform` attributes based on the input value from `d.y`.

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

Below is a screenshot of the changes I made based on the third round of feedback.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/third-feedback-implemented.png)

This was later updated to the following to address the 4th round of feedback since the information shown by the mouseover event wasn't centering equally for all 3 plots.

```javascript
function showSurvivalData(d, i) {
  svg.append("text")
    .attr({
      id: "text" + "-" + d.y + "-" + i,
      x: function() { return xScale(d.x); },
      y: function() { return yScale(d.y + d.y0); },
    })
    .attr("transform", function() {
      if (xScale.rangeBand() - 100 < 100) {
        return "translate(45, -5)";
      }
      else if (xScale.rangeBand() - 100 >= 100 && xScale.rangeBand() - 100 <= 190) {
        return "translate(67, -5)"
      } else {
        return "translate(145, -5)";
      }
    })
    .text(function() { return [d.y, ((d.y / totalPassengers) * 100).toFixed(2) + "% of Total Passengers"]; });
}
```

My final round of feedback was from my Udacity project reviewer, who recommended showing multiple passenger characteristics in the visualization and how they impacted survival status and a summary of the conclusions behind the visualization. To address this, I added 2 additional passenger characteristics, Age Group, and Gender, to the visualization with a brief summary, conclusion, and some additional findings to help tell a story to the user as they interacted with the visualization.

Technically in order to do this, I created 3 new csv files that grouped data based on the 3 variables mentioned above. These were grouped using R, and have been listed below.

  1. `grouped_by_age_group.csv`
  2. `grouped_by_gender.csv`
  3. `grouped_by_passenger_group.csv`

Once these were created, I used three different functions to load in each CSV file depending on which radio button was selected. This was done using the following event handlers.

```javascript
d3.select('#radio-age').on("click", function() {
  showAgeGroupData();
});

d3.select('#radio-gender').on("click", function() {
  showGenderData();
});

d3.select('#radio-passenger-class').on("click", function() {
  showPassengerClassData();
});
```

Afterwards, I added radio buttons above the visualization to allow users to toggle between the three different characteristics so they could be viewed one at a time. Finally, I added a small summary at the beginning of the page, a conclusion at the end including the most apparent trends, and some additional trends I found when analyzing the data in case the reader was interested.

Below is a screenshot of the final updated based on the fourth round of feedback, which is also reflective of the final `index.html` file in this repository.

![alt text](https://github.com/bljustice/titanic-survival-data-visualization/blob/master/fourth-feedback-implemented.png)

##Feedback
I reached out to 3 different people about my initial visualization and they each had different feedback, which I have listed below. I also received feedback from my Udacity project reviewer, which is summarized in the 4th round of feedback below.

  1. The bars in the visualization are too wide and don't seem centered above the x axis tick marks. Is there a way to center them and shrink them?
  2. The legend appears to be slightly cut off on the right side. Is there a way to show the full legend?
  3. Both data points that are shown by the mouseover events aren't centered. Is there a way to center these to make it more legible?
  4. Several different passenger characteristics should be shown in the visualization versus one, possibly using a radio or toggle button to switch between them. A summary and/or conclusion should be added to provide context to the visualization.

##Resources
  * https://www.kaggle.com/c/titanic/data
  * https://bl.ocks.org/mbostock/1134768
  * http://bl.ocks.org/hlucasfranca/f133da4493553963e710
  * https://bl.ocks.org/mbostock/3886208
  * http://learnjsdata.com/group_data.html
  * http://www.icyousee.org/titanic.html
