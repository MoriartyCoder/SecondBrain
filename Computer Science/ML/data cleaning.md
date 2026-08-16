

## Columns
### clean columns
#### Change Datatype
Works when there are no errors in the colum.
```python
df['INDEX'] = df['INDEX'].astype(int)
```

When there are nullvalues this is a better way:
```python
# Fill NaN values with a default value, for example, 0 
df['Age'] = pd.to_numeric(df['Age'], errors='coerce') 
df['Age'] = df['Age'].fillna(0).astype(int)
```