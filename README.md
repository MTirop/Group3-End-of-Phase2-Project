### Leveraging Multi-Platform Analytics for Smarter Film Investment Decisions
![Guy watching movies](./Images\PNG.png "Guy watching movies")

#### Table of Contents
- [Project Description](#project-description)
- [Objectives](#objectives)
- [Data Sources](#data-sources)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Contributors](#contributors)
- [License](#license)

#### Project Description

In response to the growing trend of major corporations venturing into original video content, our company is embarking on an exciting initiative: establishing a new movie studio. Recognizing our limited prior experience in film production, the company has enlisted our help as **Group 3 members** to provide essential strategic guidance.

The global film industry, a multi-billion-dollar arena, demands precise understanding of audience preferences, box office dynamics, and optimal timing for success. To navigate this highly competitive landscape effectively, leveraging data-driven insights is paramount. This project undertakes a comprehensive analysis of historical and real-time data from various platforms. Its primary aim is to uncover critical trends, audience behaviors, and key performance indicators that will inform our studio's launch strategy, enabling us to minimize inherent risks and maximize the potential returns on our initial film investments.


#### Objectives
The primary objectives of this project were to:

* Collect and consolidate movie performance data from various trusted platforms (Box Office Mojo, IMDb, Rotten Tomatoes, TheMovieDB, and The Numbers) into a unified and analyzable dataset.
* Identify key factors influencing movie success, such as genre, release timing, budget, critical ratings, and star power, through correlation analysis and predictive modeling.
* Segment successful and unsuccessful films using clustering and classification algorithms to define risk profiles and market patterns.
* Provide genre and budget recommendations tailored to emerging market trends and audience demand, supported by historical performance metrics.
* Develop an interactive dashboard or report that visualizes insights and guides decision-makers on optimal strategies for production, marketing, and release planning.

#### Data Sources
The comprehensive analysis of the movie industry was performed using data assembled from the following datasets:

* IMDb Data Base
* Box Office Mojo
* Rotten Tomatoes
* TheMovieDB
* The Numbers
These datasets collectively provide information on genres, production companies, budgets, revenues, profitability, ratings, language, and popularity scores.

#### Methodology
The project utilized Python for data analysis, employing a robust methodology that included:

* Data Cleaning and Preprocessing: Extensive cleaning was performed using pandas and numpy to ensure data consistency and usability. This involved handling missing values (dropping or imputing records with missing box office figures or studio information), fixing formatting issues (removing non-numeric characters from financial columns), and removing duplicate entries.

##### Exploratory Data Analysis (EDA) & Visualization

`matplotlib` and `seaborn` were extensively used to visualize trends, correlations, and outliers across the datasets. Key visualizations included:
* **"Production Budget vs Worldwide Gross (Colored by ROI)" Scatter Plot:** To analyze the relationship between budget, gross, and return on investment.
* **"Average Rating vs Worldwide Gross" Scatter Plot:** To explore how IMDb ratings and the number of votes influence gross revenue.
* **"Average Rating vs Number of Votes by Genre" Scatter Plot:** To understand genre-specific audience reception.
* **"Revenue Distribution by Genre (2013 vs 2016)" Box Plot:** To visualize the distribution of worldwide gross for different genres and identify top performers over time.
* **"Total Revenue by Genre (2013 vs 2016)" Bar Chart:** To compare total gross revenue across genres for specific years.
* **"Top 10 Studios by Average Worldwide Gross" Bar Chart:** To identify leading studios based on their average film performance.
* **"Top Studios in 2013 and 2016" Bar Chart:** To compare the total gross of leading studios in specific years.

* *Statistical Analysis:*
    * **One-Way ANOVA:** Performed to statistically confirm if there are significant differences in average ratings across different movie genres.
    * **Linear Regression Model:** Developed to identify and quantify the impact of various features (e.g., genre, `log_votes`, `start_year`, `runtime_minutes`, `is_original_title`) on predicting a film's `averagerating`.

#### Key Findings
###### Visualizations
* Production Budget Vs Worldwide Gross
```
plt.figure(figsize=(10, 6))
sns.scatterplot(data=tnmovie_df, x='production_budget', y='worldwide_gross', alpha=0.5)
plt.title('Production Budget vs Worldwide Gross')
plt.xlabel('Production Budget (USD)')
plt.ylabel('Worldwide Gross (USD)')
plt.xscale('log')
plt.yscale('log')
plt.grid(True)
plt.tight_layout()
plt.show()
```
![Budgets vs Worldwige Gross](./Images/Production%20Budget%20Vs%20Worldwide%20Gross.png)

This scatter plot displays the **worldwide gross revenue** of movies against their **production budget**, both on a logarithmic scale.

* **Positive Correlation:** There is a clear **positive correlation** between production budget and worldwide gross. Generally, movies with higher production budgets tend to achieve higher worldwide gross revenues. This suggests that investment in production often translates to greater financial returns, possibly due to higher quality, more extensive marketing, or bigger stars.
* **Logarithmic Scale:** The use of a logarithmic scale on both axes is crucial. It helps to visualize the wide range of values for both budget and gross, especially highlighting differences among lower-budget and lower-grossing films, while still accommodating the blockbusters. Without it, the data points for lower values would be compressed and indistinguishable.
* **Spread and Variability:** While there's a positive trend, there's also significant **variability** in the data.
    * **High Budget, Low Gross:** We can observe some points in the upper right quadrant that are further down, indicating movies with high budgets that did not achieve proportionally high gross revenues (potential box office flops).
    * **Low Budget, High Gross:** Conversely, there are also points in the lower left quadrant that extend significantly upwards, representing movies with relatively low production budgets that managed to achieve very high worldwide gross revenues (e.g., highly profitable indie films or unexpected hits). These are often referred to as "sleeper hits."
* **Concentration:** The majority of the data points appear to be concentrated along a diagonal band, reinforcing the general trend that higher budgets often lead to higher gross.

This visualization provides valuable insights into the economics of film production and helps in understanding the relationship between investment and financial success in the movie industry. Further analysis could involve examining outliers, genre-specific trends, or the impact of marketing budgets.

* Average Rating Vs Worldwide Gross
```
plt.figure(figsize=(10, 6))
sns.scatterplot(data=recent_df, x="averagerating", y="worldwide_gross", 
                size="numvotes", hue="averagerating", palette="coolwarm", alpha=0.6)
plt.title("Average Rating vs Worldwide Gross")
plt.xlabel("IMDb Average Rating")
plt.ylabel("Worldwide Gross ($)")
plt.yscale("log")
plt.legend(title="Rating", loc='upper left', bbox_to_anchor=(1,1))
plt.tight_layout()
plt.show()
```

![Average rating vs Worldwide Gross](./Images/Average%20Rating%20Vs%20Worldwide%20Gross.png)

* There is a general trend showing that as IMDb Average Rating increases, so does the Worldwide Gross, particularly for ratings above 6.
*  Movies with a higher number of votes (larger, darker bubbles) tend to occupy the higher-grossing and higher-rated sections of the plot, suggesting a correlation between broad audience engagement (more votes) and commercial success.

* Average ratings vs Number of Votes by Genre
```
plt.figure(figsize=(12, 8))

# Sample to avoid overcrowding (optional)
sample = imdf_genres[imdf_genres['genres'].isin(top_genres['genres'])].sample(5000, random_state=42)

sns.scatterplot(data=sample, x='numvotes', y='averagerating', hue='genres', alpha=0.7, palette='tab10')
plt.xscale('log')
plt.title('Average Rating vs Number of Votes by Genre (Sample)')
plt.xlabel('Number of Votes (log scale)')
plt.ylabel('Average Rating')
plt.legend(title='Genre', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.tight_layout()

plt.show()
```
![Average Rating Vs Number of votes by Genre](./Images/Average%20rating%20vs%20Number%20of%20votes%20by%20Genre.png)

* Most genres are widely distributed across the spectrum of average ratings and vote counts, indicating that popularity (number of votes) and critical reception (average rating) are not exclusive to any single genre.
* A significant cluster of films, spanning various genres, achieves high average ratings (above 6) and a substantial number of votes, suggesting a general consensus on quality for popular movies.
* While high vote counts tend to correlate with higher average ratings, the lower end of the "Number of Votes" scale (below 10^2) shows a wider spread of ratings, including many lower-rated films, possibly due to less audience exposure or niche appeal.

* Revenue Distribution by Genre
```
# Unpivot genre columns
genre_cols = [col for col in recent_df.columns if col.startswith("genre_")]
genre_df = recent_df.melt(id_vars=["ROI"], value_vars=genre_cols, 
                          var_name="genre", value_name="has_genre")
genre_df = genre_df[genre_df["has_genre"] == True]
genre_df["genre"] = genre_df["genre"].str.replace("genre_", "")

# Group and sort
genre_profit = genre_df.groupby("genre")["ROI"].mean().sort_values(ascending=False)

plt.figure(figsize=(10, 6))
sns.barplot(x=genre_profit.values, y=genre_profit.index, palette="Spectral")
plt.title("Average ROI by Genre")
plt.xlabel("Average ROI")
plt.ylabel("Genre")
plt.tight_layout()
plt.show()
```
![Revenue Distributiob Vs Genre](./Images/Revenue%20Distribution%20vs%20Genre.png)

*  Mystery, Horror, and Thriller genres exhibit significantly higher average Return on Investment (ROI) compared to all other genres.
* There's a substantial difference in average ROI across genres, ranging from over 18 for Mystery down to less than 1 for Documentary.
* Documentaries, Fantasy, and History genres show the lowest average ROI in this dataset.

* Top 10 Studios By Average Worlwide gross
```
# Filter to studios with enough movies to avoid noise
studio_perf = (recent_df.groupby("studio")["worldwide_gross"]
               .agg(["mean", "count"])
               .query("count >= 10")
               .sort_values("mean", ascending=False))

plt.figure(figsize=(12, 6))
sns.barplot(y=studio_perf.head(10).index, x=studio_perf.head(10)["mean"], palette="Blues_r")
plt.title("Top 10 Studios by Average Worldwide Gross (≥10 films)")
plt.xlabel("Average Worldwide Gross")
plt.ylabel("Studio")
plt.tight_layout()
plt.show()
```
![Top 10 Studios vs Worldwide Gross](/Images/Top%2010%20studios%20vs%20Worldwide%20gross.png)

* The presence of an Indian studio ("GrtIndia") in the top 10 suggests the significant and growing global market for specific non-Hollywood content.Explore co-production opportunities, distribution deals, or content acquisition in major non-Western film markets to tap into their large audience bases.
* Review and strengthen international sales teams, distribution partnerships, and marketing efforts in key global markets, especially emerging ones. This is because high worldwide gross implies robust international distribution capabilities.
* To increase average gross, prioritize projects that have the potential for sequels, shared universes, or resonate widely across diverse international markets.

###### Statistical Findings
* One Way ANOVA
Analysis of Variance, is a statistical test used to determine if there are statistically significant differences between the means of three or more independent groups.

```
from scipy.stats import f_oneway

# Get the ratings for each genre (as a list of lists)
genre_groups = [group['averagerating'].values for _, group in df_exploded.groupby('genres') if len(group) > 100]

# Perform one-way ANOVA
f_stat, p_value = f_oneway(*genre_groups)
print(f"ANOVA F-statistic: {f_stat:.2f}, p-value: {p_value:.4f}")
```
ANOVA F-statistic: 4106.40, p-value: 0.0000

###### Inferences
* The F-statistic is very large. It indicates that the variance between the average ratings of different genres is significantly larger than the variance within the average ratings of individual genres. In simpler terms, the average ratings for different genres are spread out from each other much more than the individual movie ratings are spread out within the same genre.

* Since the p-value (0.0000) is much, much less than common significance levels (e.g., 0.05 or 0.01), we reject the null hypothesis.
The null hypothesis: There is no statistically significant difference in the average ratings among the different movie genres.
By rejecting the null hypothesis, we conclude that there is a statistically significant difference in the average ratings among the different movie genres.
* The statistical significance of this test confirms that not all genres are perceived equally by audiences in terms of average rating. Some genres inherently tend to receive higher average ratings than others.

* Linear Regression Model
A linear regression model is a fundamental statistical method used to model the relationship between a dependent variable (also called the response or outcome variable) and one or more independent variables (also called predictor or explanatory variables) by fitting a linear equation to the observed data.
```
# Log-transform votes and total_gross
merged_df['log_votes'] = np.log1p(merged_df['numvotes'])
merged_df['log_total_gross'] = np.log1p(merged_df['total_gross'])

# Create genre dummy variables
merged_df['genres_list'] = merged_df['genres'].str.split(',')
genres_expanded = merged_df.explode('genres_list')
genre_dummies = pd.get_dummies(genres_expanded['genres_list'], prefix='genre')
genres_dummies_agg = genre_dummies.groupby(genres_expanded.index).max()
merged_df = pd.concat([merged_df, genres_dummies_agg], axis=1)

# Define features and target
feature_cols = ['runtime_minutes', 'start_year', 'is_original_title', 'log_votes', 'log_total_gross'] + list(genre_dummies.columns)
X = merged_df[feature_cols]
y = merged_df['averagerating']

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train linear regression model
model = LinearRegression()
model.fit(X_train, y_train)

# Predict and evaluate
y_pred = model.predict(X_test)
rmse = mean_squared_error(y_test, y_pred, squared=False)
r2 = r2_score(y_test, y_pred)

print(f"RMSE: {rmse:.3f}")
print(f"R^2: {r2:.3f}")

# Output coefficients
coef_df = pd.DataFrame({'Feature': X.columns, 'Coefficient': model.coef_}).sort_values(by='Coefficient', ascending=False)
print(coef_df.head(15))
```
* Linear Regression Coefficient BarChart
![Linear Regression Coefficient Barchart](./Images/Linear%20regression%20Coefficient%20Bar%20Chart.png)

- Popularity (log_votes)also has a strong positive effect on average ratings, indicating that movies with more votes tend to have higher ratings.
- The variable is_original_title has a small positive coefficient, suggesting original titles may slightly correlate with higher ratings.
- Runtime and start year  have small positive effects on ratings.
- Some genres such as Crime and Sci-Fi show a slight negative association with average ratings in this model.
- Encouraging high engagement and votes (through marketing or distribution) may positively impact perceived movie quality.
- While box office revenue matters, genre and audience engagement seem to play more critical roles in driving average ratings.

#### Recommendations
* Prioritize High ROI Films: Focus on lower-budget projects with strong audience appeal to maximize return on investment.
* Diversify content: Given revenue volatility, a diversified genre and release strategy can hedge risk and maintain steady performance.
* Ensure Quality and Audience Engagement: Invest in strong content and marketing to achieve high IMDb ratings and generate significant audience buzz.
* Enter Niche Markets Strategically: Build expertise in specific genres or types of films before attempting broad market penetration.
* Explore co-production opportunities, distribution deals, or content acquisition in major non-Western film markets to tap into their large audience bases.
* Implement Smart Marketing & Distribution: Develop cost-effective and targeted strategies to ensure films reach their intended audience.
*  Adventure, Action, Animation, Sci-Fi, and Fantasy consistently demonstrate the highest median worldwide gross across both 2013 and 2016. These genres also contain the majority of the highest-grossing outlier films.
*  "BV" (Buena Vista/Disney) consistently dominates both average worldwide gross per film and total gross, driven by strong franchise development and global marketing. Other major studios like Universal (Uni.), Warner Bros. (WB), and Fox also demonstrate consistent high performance.
* Implement rigorous financial planning for each project. Balance a few larger-budget, high-potential "tentpole" films (likely in the top-performing genres) with more numerous, lower-budget, high-ROI projects to diversify risk and stabilize revenue
* Studios like BV (Buena Vista/Disney), Universal, and Warner Bros. consistently lead in both total gross and average gross per film, often due to their strong IPs, franchise development, and global marketing reach. Therefore focusing on creating high-quality content in popular genres while simultaneously exploring unique IPs, co-production opportunities, or talent-driven projects that offer a competitive edge without requiring immediate mega-budgets is important.
* Consider the global appeal of your film ideas. Develop robust international distribution strategies and marketing plans tailored to diverse markets to maximize worldwide box office potential because the success of top genres and studios relies heavily on worldwide gross, indicating the importance of international markets.
* Preparedness for market dips, and building flexibility into budgets and marketing plans to withstand underperforming years.
* Continuously monitor industry trends, audience preferences, and emerging technologies. Be prepared to adapt production strategies and genre focus based on real-time market data.

#### Contributors
* Lydiah Chumba
* Dennis Ochieng Ouma
* Yvonne Makena
* Brian Kipchumba
* Kimberly Koskei

#### Licence
