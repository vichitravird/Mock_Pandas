# Mock_Pandas

## 1 Problem 1 : Not Boring Movies (Mock Problem) (https://leetcode.com/problems/not-boring-movies/)
<br>
import pandas as pd
<br>
def not_boring_movies(cinema: pd.DataFrame) -> pd.DataFrame: <br>
    df=cinema[(cinema['id']%2!=0) & (cinema['description']!='boring')] <br>
    return df.sort_values('rating', ascending=False)


## 2 Problem 2 : Biggest Single Number (Mock Problem) ( https://leetcode.com/problems/biggest-single-number/ )
<br>
import pandas as pd

def biggest_single_number(my_numbers: pd.DataFrame) -> pd.DataFrame:
    return my_numbers.drop_duplicates(keep = False).max().to_frame(name = 'num')
<br>
import pandas as pd

def biggest_single_number(my_numbers: pd.DataFrame) -> pd.DataFrame:
    #Group by all the numbers and get a count
    #Filter out all the numbers where count is 1
    #Show the max number

    df=my_numbers.groupby(['num']).size().reset_index(name='count')
    df=df[df['count']==1]
    df=df.sort_values('num', ascending = False)
    df=df[['num']].max()
    df=pd.DataFrame(df)
    return df.rename(columns={0:'num'})

<br>

## 3 Problem 3 :Sales Analysis III (Mock Problem) (https://leetcode.com/problems/sales-analysis-iii/)

<br>
import pandas as pd

def sales_analysis(product: pd.DataFrame, sales: pd.DataFrame) -> pd.DataFrame:
    merged_df=pd.merge(product, sales, on='product_id')
    df_inside=merged_df[(merged_df['sale_date']>'2019-01-01') & (merged_df['sale_date']<'2019-03-31')]
    df_outside=merged_df[(merged_df['sale_date']<'2019-01-01') | (merged_df['sale_date']>'2019-03-31')]
    df=product[(~product['product_id'].isin(df_outside['product_id'])) & (product['product_id'].isin(df_inside['product_id']))]


<br>

## 4 Problem 4 : Market Analysis I (Mock Problem) ( https://leetcode.com/problems/market-analysis-i/) 
<br>
import pandas as pd

def market_analysis(users: pd.DataFrame, orders: pd.DataFrame, items: pd.DataFrame) -> pd.DataFrame:
    orders = orders[["order_date", "buyer_id"]]
    users = users[["user_id", "join_date"]].rename(columns={"user_id" : "buyer_id"})
    orders = orders.loc[orders["order_date"].dt.year == 2019].groupby(["buyer_id"])["order_date"].count().reset_index()
    return users.merge(orders.rename(columns={"order_date" : "orders_in_2019"}), how="left").fillna(0)
