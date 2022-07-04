# quantaverse_test
Interview Assignment Test

Kindly check the Take_Home_Assignment.ipynb file 

to run the file in commandline use following file data_processing.py
* This file is using prefect for better logging 
* Good to explore about this and can be created Parallel tasks
  https://orion-docs.prefect.io/tutorials/first-steps/
  
Make sure you are using Python3.8 onwards 


Approach 

1. Read the transactions_medium.csv file using Pandas 
	There is  better way we can use pyspark/spark for data crunching also.

2. I have designed a function dt_inplace which will read a given transactions_medium.csv file and automatically detects and converts each column whose datatype
    is 'object' to a DateTime and returns the resulting dataframe.
	- the reason behind using this approach to save time on conversion of object to datetiem 
	- Used type annotations python3.8, in the function definition and in returned mentioned datatype. This helps to performance benefits also because of explicit mentioning of data type

3. sort data on the basis of TIMESTAMP column in ascending order 

4.  There are few data transactions where the sender and receiver are the same, so drop those transactions 
	if those transactions need to be included only on condition needs to be changed.
	What I assumed is how sender and receiver can be same, so ignored that transac
  
 |                               TRANSACTION_x                              |     TIMESTAMP_x     | AMOUNT_x |     SENDER_x     |    RECEIVER_x    |
|:------------------------------------------------------------------------:|:-------------------:|:--------:|:----------------:|:----------------:|
| f7fa68c9abea4c7dcc812245da827483a772ee42fa459279965409da1416b2e620100227 | 2010-02-27 10:55:12 | 160.0000 | ID00000000000000 | ID00000000000000 |
| f7fa68c9abea4c7dcc812245da827483a772ee42fa459279965409da1416b2e620100227 | 2010-02-27 10:55:12 | 160.0000 | ID00000000000000 | ID00000000000000 |
| f7fa68c9abea4c7dcc812245da827483a772ee42fa459279965409da1416b2e620100227 | 2010-02-27 10:55:12 | 160.0000 | ID00000000000000 | ID00000000000000 |
| f7fa68c9abea4c7dcc812245da827483a772ee42fa459279965409da1416b2e620100227 | 2010-02-27 10:55:12 | 160.0000 | ID00000000000000 | ID00000000000000 |
| f7fa68c9abea4c7dcc812245da827483a772ee42fa459279965409da1416b2e620100227 | 2010-02-27 10:55:12 | 160.0000 | ID00000000000000 | ID00000000000000 |


5. designed data_cleaning function which accepts dataframe, window_size and returns dataframe
	- grouped the data based on window_size { window_size (in days, 0 being same day and 3 being 3 day window)}
	- made inner join on the same dataset to find the same 	bridge transactions
	- checking if the bridges are found then create a new dataframe to accumulate that data 
		- if the bridge found iterate on it to check if that bridge is the actual bridge by verifying bridge_fee 0.01 to 0.10 is taken by the bridge person
			-	if year then count those bridge transactions
			- 	append the data in the list 
	- at the end assign a new column and value of total bridges to those transactions 
	- append all the final_df and tmp_df data to be outputed at the end
		
6. on the received dataframe group the bridge persons and sort it based on the count of bridges in descending(highest to lowest) order 
