# CO2 Forecasting Model
Modeling and forecasting atmospheric CO₂ from 1958 until 2058

Since 1958 atmospheric carbon dioxide measurements have record at the Mauna Loa Observatory in Hawaii. CO2 levels have been increasing steadily since the start of the industrial revolution in the 18th century.
Older data are from ice core measurements, not atmospheric measurements. The data from Mauna Loa provide very direct data on atmospheric CO2, which forms an important part of global climate change modeling.

This code creates a statistical model that explains this data set, uses it to forecast what measurements will look like between now and the start of 2058 — 40 years from now. The model reflects the uncertainty in the predictions, showing confidence intervals. The model also predicts when we are likely to reach high risk levels of CO2 with greater probability for serious climate change. The models results mirror forecasts made by the 2013 Low Carbon Economy of reaching High Risk levels of CO2 concentration (450 ppm) by the year 2034.

# The Data
Data was downloaded from 
The weekly Mauna Loa data set has 2 columns — the date and the measurement of CO2 ppm (parts per million). The day integer, day_int, column was added for ease of modelling. Dates are processed as days since the first measurement. For modeling purposes I represent time, t, using the number of days since measurements started in 1958, and the measured value, xt, as the CO2 ppm measurement. At the time of modelling, there were 3044 values in the data set. They still make measurements every week, so this data set will keep growing in size.

# The Model
Three temperature components are modeled - an overall trend, seasonal variations and noise. Parameters are identified, priors are put over them, and posterior distributions are calculated to predict what atmospheric CO2 levels will look like up to the start of 2058.

Long-term trend: quadratic, c0+c1(t)+cs2(t^2)
Seasonal variation (every 365¼ days): cosine, c2(2 pi t / 365.25+c3)
Noise: Gaussian with 0 mean and fixed standard deviation, c4^2
The ci variables are all unobserved parameters of the model.
Combining these three components gives the following likelihood function:

![equation](http://latex.codecogs.com/gif.latex?p%28x_%7Bt%7D%7C%5Ctheta%29%20%3D%20N%28c_%7B0%7D%20&plus;%20c_%7B1%7Dt%20&plus;%20cs_%7B2%7D%28t%5E2%29%20&plus;%20c_%7B2%7Dcos%282%5Cpi%20t/365.25%20&plus;%20c_%7B3%7D%29%2Cc_%7B4%7D%5E2%29)

where θ represents the set of all unobserved parameters. Since there are 3044 data, the full likelihood comprises a product over all 3040 values, xt. To complete the model I define priors over all 6 model parameters.
