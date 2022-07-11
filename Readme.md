
## Stock Price Prediction:

Stock Price Prediction based on multiple stacked LSTM ( deep learing method ). We have processed the whole data on the basis on the closing price of the stock on each day from 1980 to 2022.
The Dataset contains AAPL (Apple stocks) with their open price , closing price , volume of stocks and adj_close for every day the market was open since 1980.
Dataset was taken from Kaggle , inlcuding of 10468 rows and 7 columns.



## Pre-Processing:
1. We started of with the Pre-Processing with the reshaping of the dataset of column 'Close" into a range of -1 to 1. To work on the Data better and easily.

2. Now what we do in a general dataset is divided it into train and test split to work on the data accordingly , but this is a Time Series Data in which each day's stock will dpend on the previous day's data and we cannot go on splitting this data into random splits. So In case of time series data , we shouldnt split the data based on Cross Validation or Random Seed. We should split the test on the basis of different days as explained. We cannot take Random days Stock's and train with it for the Future

3. So we split the data as 65% and 35% , starting training split will be given as 65% and rest 35% for test split. Leaving 6804 for the Training Split and likewise 3664 for training split.

4. Now comes the important part of making X_train , Y_train , X_test and Y_test which we do on the basis on the time_steps.

Lets say we have a time_step of 3 meaning we require previous 3 days of stocks to predict the 4th day.
For Example Stocks for days are as follow:

Day 1 --> 130

Day 2 --> 134

Day 3 --> 132

Day 4 --> 137

Day 5 --> 138

Day 6 --> 140

So For Day 1 , 2 , 3 give us the output of Day4. Likewise we require data of Day 2 , 3 , 4 for Day5.

So our X_train will be our data of the day's we need to provide as output & X_test will be the outputs we get from the data we provide of the days.
We train our Y_test and Y_train in the same way.

We do this in our dataset with a time_step of 100 ( Meaning it requires the data of previous 100 days to calculated the next day's output)









So we get our X_train and Y_train with 100 columns and our X_test and Y_test with 1 column of output.



## Stacked LSTM

First answering the question of why choose LSTM's?

LSTMs have an edge over conventional feed-forward neural networks and RNN in many ways. This is because of their property of selectively remembering patterns for long durations of time

*Working on Stocks requires us to work with*

1.The trend that the stock has been following in the previous days, maybe a downtrend or an uptrend.

2.The price of the stock on the previous day, because many traders compare the stock’s previous day price before buying it.

3.The factors that can affect the price of the stock for today. This can be a new company policy that is being criticized widely, or a drop in the company’s profit, or maybe an unexpected change in the senior leadership of the company.

*These dependencies can be generalized to any problem as:*

1.The previous cell state (i.e. the information that was present in the memory after the previous time step)

2.The previous hidden state (i.e. this is the same as the output of the previous cell)

3.The input at the current time step (i.e. the new information that is being fed in at that moment)


In our code we made 4 layers which included 3 LSTM layers and a Dense Layer containing of 50 inputs.

After making out layers , we fit our model into our X_train and Y_train with 100 epochs and keeping X_test and Y_test as our validation_data.



## Prediction:

We add a new variable of Train_predict and test_predict fitting the model with X_train and X_test.

To predict the stocks of a new day we minus 100 (Time_steps) and feed it to x_input which after reshaping is added into temp_input.

After that we make a code for a loop of 30 days which takes 100 days from the temp_input and fits it to the model and gives us an answer for the stock of the following day. Then for predicting the data of 2nd following day it will start from the 1st position (discarding the first data of day1 and starting from day2).

We add it to a list called as lst_output and if we check the temp_output it has 101 values (indicating the prediction from the LSTM for the further day).
While lst_output has 30 days full of predicitons of the stocks.
