# Video-Game-Data-PBI-DEMO
*Using Video Game data to display insights*

## Purpose
This dashboard is an adaptation of a Power BI report I created as a data analyst to advertise to colleagues within my company interested in Power BI as a data analysis tool. This demonstration shows a brief capacity of design, data preparation and cleaning, and visualization of data to lead to insights.

I have recreated this for my personal use to showcase my Power BI developer prowess and to add to my portfolio, and by using public data, do not disobey any restrictions or security concerns from my employer.

## Data
The data used is public data from [Kaggle](https://www.kaggle.com/datasets/sidtwr/videogames-sales-dataset). The data contains information regarding video game sales from 1980 - 2020 (*no data from 2018 and 2019*). The data source utilized from this site was the Video_Games_Sales_as_at_22_Dec_2016.csv file. An additional column "Franchise" was created using significant data querying in Power BI to extract franchise names from the Game Titles.

The Sales columns were split into four columns in the original dataset to represent the different regions of sales for North America ("NA_Sales"), Japan ("JP_Sales"), Europe ("EU_Sales"), and all other regions ("Other_Sales"). In addition, there was a sum column of the four regions, "Global_Sales." The Global_Sales column was removed in data preparation as it is redundant and can be replaced with a measure if a total sales is required. The four regions' sales columns were pivoted into two columns: a Region column and a Sales column, for a better data model and flow of information.

The end result of the fields include: 
- Game Title
- Franchise
- Year (of game release)
- Genre
- Publisher
- Developer
- Platform (gaming system)
- Rating (ESRB, etc.)
- Sales
- Critic Score
- Critic Count
- User Score
- User Count

## Measures

### Copyright
Although a copyright statement is not required for this usecase, a *fake* copyright type message is displayed to mimic that of a typical Power BI report. 

```
copyright = "© " & YEAR(TODAY()) & " Video Game Data via Kaggle. Demonstration created to showcase Power BI report design."
```

The ```YEAR(TODAY())``` code is utilized to create a dynamic year in the copyright statement.

### Score Averages (Critic and User)
In Power BI, you can directly bring in implicit (numbers identified by Power BI) fields into your visualizations and right click them to determine if you want a sum, average, minimum, maximum, first, last, and so on values of that field. However, the best practice is to instead create explicit values (created measure) to make these aggregations. In doing so, you improve performance of your report and create these aggregations to occur on an as needed basis.


```
Critic Score Avg = AVERAGE('Video Game Sales Data'[Critic Score])
User Score Avg = AVERAGE('Video Game Sales Data'[User Score])
```

Furthermore, an "Overall Score" measure, which is essentially an average, was created manually, which results in the average of the user and critic score. 
```
Overall Score = divide(([User Score Avg])+[Critic Score Avg],2)
```

Since the scores at a game level were already provided to be their average score from the critic and users counts, it is not possible to determine the true combined overall score and is estimated. In a best case scenario, the rating given by each user and critic would be provided, and a total division of all of the critic and user scores by the total number of critics and users would be performed for an accurate overall score.

### Totals
Under the same justification explained in the [Score Averages](https://github.com/sachsac/Video-Game-Data-PBI-DEMO/edit/main/README.md#score-averages-critic-and-user) section, to calculate totals, or sums, an explicit measure should be utilized. Below are the examples of the explicit measures created using ```sum()```.

```
Total Critic Count = sum('Video Game Sales Data'[Critic Count])
Total Sales = sum('Video Game Sales Data'[Sales])
Total User Count = sum('Video Game Sales Data'[User Count])
```

*Note, for the User and Critic counts, in a perfect world it would be better to do a distinct count on a users or critic list if provided. However, due to the set up of the data, we did not have access to information to dictate whether critics/users were unique or not. Therefore, artistic liberties were taken in the creation of these measures and report to assume that they are all unique (which is not realistic).*


### Greeting
At the top right corner of the report, the viewer of the report receives an individualised greeting, which will display the logged in username.

```
Hello = "Hello, " & USERNAME()
```

If a list of users is provided, this can be further customized to something like the below code to further personalize with a first name.

```
Hello = 
var user = USERNAME()
var display = LOOKUPVALUE(User[First Name], User[Email], user)
var prefix = "Hello ,"
RETURN
CONCATENATE(prefix, display)
```

## The Report
1. ![image](https://user-images.githubusercontent.com/86759538/209222931-b35be1ec-23de-46db-a36d-3c6292bd49ab.png)
Aspects of this page:
- Welcome page, gives basic details regarding the report

2. ![image](https://user-images.githubusercontent.com/86759538/209223040-25a4d51a-fd10-4e58-9a9d-6606d5e81ea7.png)
Aspects of this page:
- Slider (filter) for viewers to determine which date range they would like to see data for. 
  - Default is the entire time range available
- Page Filter for Regions

3. ![image](https://user-images.githubusercontent.com/86759538/209225975-c78cabd4-c837-41fd-8ffa-13b8b6011d6a.png)
Aspects of this page:
- Bar chart of the top 10 video games, with color splitting how much each Region contributed to that total
- Cards to show which game was the top selling game in each specific region (to save people from having to filter or individually check each game)
- Page Filter for Regions

4. ![image](https://user-images.githubusercontent.com/86759538/209226585-9c12e60f-2372-4440-85a0-e58e3f74b1b1.png)
Aspects of this page:
- Bar chart of the top 10 franchises, with color splitting how much each Region contributed to that total
- Cards to show which game was the top selling franchises in each specific region (to save people from having to filter or individually check each franchise)
- Page Filter for Regions

5. ![image](https://user-images.githubusercontent.com/86759538/209226690-10c04e16-d6ce-45aa-88e4-b30de259f282.png)
Aspects of this page:
- Bar chart displays all genres, with color splitting how much each Region contributed to that total
- Cards that show the top genre in Japan (as that differed from the other regions), a Global top selling genre (which was top for all Regions except Japan, and overall), the top genre by critics (which genre had the highest rated games), and top genre by users (which genre had the highest rated games)
- Page Filter for Regions

6. ![image](https://user-images.githubusercontent.com/86759538/209226964-300b854c-f9a6-41b4-ad5b-2d24d00a8d02.png)
Aspects of this page:
- Multi-row cards for the best rated and worst rated games per critics
- Cards with highest rated game, platform with the highest rated games, developer with the highest rated games, publisher with the highest rated games, average critic score overall, total count of critics, the year with the highest average of game scores, and the region with the lowest scores by critics.
  - Critics were chosen to show the "harshest" as it is expected that critics will be very critical of games, as it is their job
- Page Filter for Regions

7. ![image](https://user-images.githubusercontent.com/86759538/209227337-97bbd613-24fd-42a1-b686-f9a99dfef630.png)
Aspects of this page:
- Multi-row cards for the best rated and worst rated games per users
- Cards with highest rated game, platform with the highest rated games, developer with the highest rated games, publisher with the highest rated games, average user score overall, total count of users, the year with the highest average of game scores, and the region with the lowest scores by users.
  - Users were chosen to show the "happiest" as the users are the customer, and it is interesting to know where the happiest customers are
- Page Filter for Regions

## Other Best Practices
Some other approaches are done within the report design for best practices.
- Hiding visual level filters
  - Filters on visuals should be hidden so that users cannot change the purpose of that visual
  - This may not always be applicable if there is a use case for users to change what the visual will show
- Colors
  - Colors are used with a purpose. 
    - The colors for the different regions are consistent
    - The colors used for the different regions are not used to represent anything else
  - The theme of the colors used throught the report are reminiscent of neon and arcade games
  - Dark background is "dark mode" friendly
    - Light text color is used for contrast
- Positioning
  - Items on report that are similar or exactly the same are positioned on the report in the exact same layout for consistency
- Star Schema
  - The data model is created to be a star schema
     - A star schema is when all dimension tables have a direct, one-to-many, and unidirectional relationship
        - This is the best model to use for optimal performance of your report
