Use **Jupyter Notebook**!

| Action            | Icon                                  | Keyboard Shortcut |
| ----------------- | ------------------------------------- | ----------------- |
| Save              | Floppy Disk                           | CTRL + S          |
| Run Cell          | Play Button                           | Shift + Enter     |
| Run all Cells     | Two play buttons alongside each other | N/A               |
| Insert Cell Below | Rectangle with an arrow pointing down | B                 |
| Delete Cell       | A trash can                           | D                 | 

You can also *move cells* by *dragging* them!

# Code Explanation
In pandas, a series is similar to a singular column in a table. It uses a *key-value pair*.
**Dataframe** has multiple rows and columns.

```python
import pandas as pd
data = [['Ben', 24, 'United Kingdom'], ['Jacob', 32, 'United States of America'], ['Alice', 19, 'Germany']]
df = pd.DataFrame(data, columns=['Name', 'Age', 'Country of Residence'])
print(df)
```

| Name              | Age                                   | Country of Residence     |
| ----------------- | ------------------------------------- | ------------------------ |
| Ben               | 24                                    | United Kingdom           |
| Jacod             | 32                                    | United States of America |
| Alice             | 19                                    | Germany                  | 


## Read from CSV
```python
df = pd.read_csv('csvfile.csv')

# Count number of columns
df.count()

# Count number of entries for each value in a specific column
df.groupby(['Columnname']).size()
```

