# Policy for SRL Column Inclusion


### Constraints

- SRL Computation happens in the interval of 1 hour gap.
- Force Indexing can be done when the user clicks ask-zia but this also need to be limited over a time interval
- Needs to force index everytime (without limit) admin clicks the compute srl
- Overall Limit : **5 million per workspace**
- Per Column Limit: **100,000** or **10,000**


### Policy

The below flow is executed in the cstore nodes during srl computation.

![Alt text](<Screenshot from 2023-07-24 12-52-22.png>)

1. All SRL Needed Columns are all columns that have ISNEEDEDFORSRL set to 1
2. Column Invalidation is done based on the number of distinct values in the column and the maximum length of the values in the column.
3. Score based priority sorting is done for the columns.

$$
Score(C{i}) = [0.2 * (count(C{i})/disinctCount(C{i}))]  + [0.5 * (ColumnPriority(C{i}))] + [3 * (UsageCount(C{i}))]
$$


4. When the user tries to include a column for ask zia, before updating the ISNEEDEDFORSRL flag for the column, need to check if the operation is allowed based on the distinct count of the column.

5. By default, All columns will have ISNEEDEDFORSRL set to 1