1.select in 操作：

in操作符可以允许在where 子句中规定多个值：

SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2...)

Eg: SELECT * FROM person WHERE Lastname IN ('Yuki','Shirley');



2.SQL BETWEEN 语法

```
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

例子：

包含：SELECT * FROM person WHERE Lastname BETWEEN 'Yuki' AND 'Shirley'

不包含：SELECT * FROM person WHERE Lastname NOT BETWEEN 'Yuki' AND 'Shirley'



3.Alias （别名）的用法：

```dart
SELECT po.OrderID, p.LastName, p.FirstName
FROM Persons AS p, Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
```

