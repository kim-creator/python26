import pymysql

import pandas as pd

import csv

​

# MySQL와 연결

conn = pymysql.connect(host='localhost', user = 'root', password='ghdekdan45', db='pythonstudy', charset='utf8')

cursor = conn.cursor()

​

# DataFrame Framework 만들어놓기

df = pd.DataFrame(columns=['id', 'name'])

​

# DataFrame에 넣어주기

for i in range(2):

id = input('ID 입력:')

name = input('이름 입력:')

df.loc[i,:] = id, name

​

# DataFrame to CSV

# https://rfriend.tistory.com/252

​

df.to_csv('C:/Workspace/Python-Study(IPA)/210605/test.csv',

sep=',',

na_rep='NaN', # 혹시 결측값이 있으면 NaN(Not a Number)로 표기 

index = False, # do not write index

header=False) # do not write columns

# columns = ['ID', 'X2'], # columns to write

# float_format = '%.2f', # 2 decimal places

​

# csv file 읽어오기

# http://pythonstudy.xyz/python/article/207-CSV-%ED%8C%8C%EC%9D%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

​

f = open('test.csv', 'r', encoding='utf-8')

reader = csv.reader(f)

​

for line in reader:

# line은 list type

tmp_id, tmp_name = line[0],line[1]

​

# DB에 넣어주기

sql = "INSERT INTO idname(id,name) VALUES(%s,%s)"

cursor.execute(sql,(tmp_id, tmp_name))

conn.commit()

f.close() 

​

# 방금 들어간 값 연동확인

sql = "SELECT id,name FROM idname WHERE id = %s"

cursor.execute(sql,('hohoho')) # id.get()는 str

# fetchone 함수로 cursor에 담겨진 하나의 데이터를 가져오기

result = cursor.fetchone() 

print("*****************************")

print(result)

print("*****************************")
