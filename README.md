# Airbnb — Agent of Gentrification? Effects of Airbnbs on Cities and Determinants of Successful Listings 

![image](https://user-images.githubusercontent.com/50972659/130538682-7518b0b0-6cb1-4809-a00a-0de9f2a2869a.png)

Airbnb is an American company based out of San Francisco that operates as an online marketplace for various forms of lodging. What's game changing about AirBnb is that is departs from the business models of traditional lodging where large corporations own and provide rentals for a given rate; Airbnb offers private homeowners the platform to list their own proerties for rent for a given period of time, giving homeowners a new way to add value to their property. This business model has proven astonishingly successful given Airbnbs rate of growth, but what is the impact on our cities? More importantly, is there anything that we can suggest to help circumvent any unwanted side effects of Airbnbs's meteoric rise to success? Lastly, what makes a successful, profitable Airbnb that can benefit both hosts and the community?

New York City has the most available data going for it, as procured by Inside AirBnB, StreetEasy, and the NYC Open Data website, so we use New York City as a case study to get an idea of how Airbnb effects society on the largest stage, and the findings here act as a sprinboard to launch similar studies for different cities.

The code and data documented in this repository is used to...

  - Import data from InsideAirbnb to get information on current listings, historical reviews, etc.
  - Set up the Connection to a Google Cloud SQL server to store & query data
  - Breakdown where Airbnbs are currently located and what's available (airbnb_effects_on_cities.ipynb)
  - Analyze Airbnb's growth in NYC neighborhoods (airbnb_effects_on_cities.ipynb)
  - Bring in Data Extracted from NYC Open Data to control for external factors (airbnb_effects_on_cities.ipynb)
  - Analyze the effects of Airbnb on rental prices via OLS regression (airbnb_effects_on_cities.ipynb)
  - Offer Suggestions to Policy Makers (airbnb_effects_on_cities.ipynb)
  - Explore what makes a successful Airbnb (predicting_airbnb_price.ipynb)
  - Predict Airbnb Prices (predicting_airbnb_price.ipynb)
  - Predict Determinants of Successful Airbnb Listings (predicting_airbnb_price.ipynb)

## Tools

A requirements.txt file is provided in the Github for any specific dependencies you may be curious about. A breakdown of our tech stack is as follows:

  - Python (Version 3.7 necessary for geopandas dependencies)
  - Visualization: Folium for choropleth, altair for interactive tables, matplotlib for the rest
  - Data manipulation: Pandas & numpy, geopandas to merge shapefiles, Stargazer to report OLS results
  - Models: scikit-learn for PCA, statsmodels for OLS regression
  - Jupyter Lab
  - Google Cloud PostgreSQL Server, optional PGAdmin4 interface
  - Psycopg2 and sqlalchemy to interface with Pandas

## Datasets

We carried out our analysis using three main data sources. The first data source was Inside Airbnb, a website that stores data on Airbnb units from certain cities across the world. Our analysis focuses on New York City, so we accessed the current (as of 07/04/2021) listings of Airbnb units. These included information on features of each listing, such as number of beds and bedrooms, as well as a list of available amenities, such as a television or a pool. Additionally, we used the calendar of availability for New York City listings. This included availability, price, minimum/maximum number of nights, and the listing ID for each date in the next calendar year. Inside Airbnb data can be located here: http://insideairbnb.com/

The second data source is the New York City Open data site, which holds a variety of publicly available data from the city government and partners. This site contains everything from records of traffic collisions to pet licenses. The data we pulled was specifically pertaining to housing, demographics, economics, crime, and social data The specific data that was used can be found here: https://www1.nyc.gov/site/planning/planning-level/nyc-population/american-community-survey.page

The third data source is StreetEasy. StreetEasy, owned by Zillow, is a New York based site that aids in renting, buying, and selling real estate in the city. The site also includes a blog and resources for understanding the real estate market. We used this site to access data on the median monthly rental prices in New York City by borough.
https://streeteasy-market-data-download.s3.amazonaws.com/rentals/All/medianAskingRent_All.zip (link to the actual file)

## Growing Airbnb Effects

Inside Airbnb's detailed listing information can be used to provide a wide range of summary statistics and is a good place to start for an exploratory data analysis. The features that it covers which is relevant to this analysis includes the following:
  - Borough
  - Neighborhood
  - Property Type
  - listing id

Altair is used to provide a faceted bar chart that visualizes a breakdown of the number of different property types available in each borough below. 36,905 listings are available in New York City. The boroughs with the most listings for all types are NYC, Manhattan, Brooklyn, Queens, Bronx, and Staten Island in that order, with the exception of Brooklyn having slightly more private rooms than Manhattan. Queens, Bronx, and Staten Island notably have far less listings than the rest. Entire homes and apartments are the most available, accounting for 19,213 listings, but private rooms are close to follow with 16,709 listings. This is relevant to our analysis because these very well may be listed for long-term rentals. Airbnb competes with the rental market, and the effect it felt on the housing supply.

![image](https://user-images.githubusercontent.com/50972659/130540156-5e40f6f2-66ff-494c-bffc-f19cd7adc48a.png)

To break down the information in the above table further and get a neighborhood level overview, geopandas and folium is used to create an interactive choropleth, with a tooltip displaying the listings available by neighborhood. Google docs cannot render the html. So a link to the choropleth is provided: https://nbviewer.jupyter.org/github/Cody-Lange/Airbnb-Captsone/blob/main/current_airbnb_nyc_map.html
![image](https://user-images.githubusercontent.com/50972659/130540215-d821139d-5d62-4bce-8526-f007e8f8d8d0.png)

From the choropleth, it can be gathered what should be fairly obvious: the downtown and touristy areas are hot spots for Airbnb listings. Hence, Manhattan, Brooklyn ,and the west side of Queens are the biggest boroughs for Airbnb. Staten Island is the least populated borough, so it follows that it has the least Airbnb listings. For an expansion of this analysis, it would be interesting to juxtapose demographics data by neighborhood to see if different cultures, races, etc. are disproportionately affected by the rise of Airbnb.

Understanding the growth rate of Airbnb is essential to understanding its effect. Inside Airbnb calendar listings, unfortunately, are only for this year and the next, so we have to use the number of historical reviews as a proxy for the number of Airbnbs being listed. This somewhat weakens the analysis of the OLS regression later, but it is logical to make the assumption that a growth in the number of Airbnbs being reviewed is an indicator for growth in the number of actual listings. 

Altair is used to make a bar chart juxtaposing the number Airbnbs being reviewed monthly with the monthly median rental prices dating back to 2010 by Borough. In the code, interactivity is added in the form of drop down bars to free up space. The charts are shown below.

![image](https://user-images.githubusercontent.com/50972659/130540345-402ac131-2a9c-4991-bbee-bcb310837648.png)

A few trends can be observed from the above plots. First of all, the number of airbnb reviews seem to follow seasonal trends with local highs around the start of the 2nd and 3rd quarter, possibly being attributed to spring and summer vacations. Secondly, median rental prices are generally creeping up from a long-term perspective, but the seasonal highs and lows correspond to that of the reviews. The long-term creep upwards can be attributed to many things, but it is interesting to note that housing of all kinds is affected on the seasonal level. However, this is not enough evidence to say that Airbnb listings have a causal relationship with rental prices. To check for this causal relationship, we must account for potential confounders, such as changes in income, available housing, education levels, crime, demographics, social data, and more. To do this, the NYC Open Data is mined for information pertaining to just that. Data from the American Community Survey are provided, but manual Excel work is necessary to stitch the data together into one in the necessary format due to the complex nature of the CSVs given. Additionally, the data is admittedly faulty in that it only gives this external data on the yearly level, while we’re trying to estimate the effect on the monthly level, so the assumption is made that the data is uniform on the monthly level in a given year. 308 features are extracted from the data and found in a CSV aptly named, ‘Literally_Everything.csv.’ A brief glimpse into what’s provided is shown in the snippet below.

![image](https://user-images.githubusercontent.com/50972659/130554900-21fb442f-34da-421e-8cb4-67d207def6ea.png)

Dealing with 300+ features, however, is a multicollinearity nightmare, with different features competing to explain an effect on the dependent variable, and we are interested in only the effect of the airbnb reviews on the median rental prices. Hence, the data is scaled via standard scaler and decomposed via principal component analysis into 10 principal components that explain 95% of the variance in these features. Ordinary Least Squares (OLS) regression is employed via the statsmodels library to help understand the the effect of Airbnb reviews on each Borough and NYC in general. The findings are summarized via the Stargazer table below.

![image](https://user-images.githubusercontent.com/50972659/130540388-a305b3f9-c36c-4341-93ed-21e8056c902d.png)

From the regression, it is found that the number of Airbnbs reviewed monthly has a statistically significant effect on the median rental prices in NYC and all Boroughs except Staten Island with moderate-to-strong R2 values between 0.7 and 0.87. This is established by having p values below the significant threshold that mean that the null hypothesis can be rejected that there is no effect. However, as reviews are a proxy for listings, further experiments should be conducted to get a more direct effect estimate. For now, the estimated effects can be explained that each review in the statistically significant columns effect the rental prices by a magnitude of a couple of pennies towards 27 cents in Bronx.

But what specifically about these listings is causing a positive effect on rent? Is it the short or long term listings? For this next regression, short-term is defined as less than 30 days, nd long term is defined as more than 30 days. Results are shown below.

![image](https://user-images.githubusercontent.com/50972659/130540420-1743654a-c7b6-4798-aeab-6f08fdee8be8.png)

From the regression table, it can be seen that the effect of long-term listings is statistically significant for NYC, Bronx, and Brooklyn with estimated positive effects on rent. The short term listings are statistically significant for only NYC and Brooklyn, but the estimated effect is actually negative.
Policy Recommendations So what does this mean in terms of policy? Per the OLS models, we can statistically prove that AIrbnb reviews, as a proxy for actual listings, have a statistically significant effect on slightly raising median rental prices in NYC as a whole, as well as all Boroughs except Staten Island. In particular, listings over 30 days are problematic. Long term Airbnb listings compete with the regular housing and rental market, so one suggestion could be to impose a limit on the length of Airbnb listings. Additionally, although taxes may be a taboo subject, special fees for long-term listings can also be explored, with the funds being funneled back towards the city.


## Successful Airbnb?
