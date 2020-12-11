# HXL Proxy Transformation Pattern Library


## Formatting Columns


### Format column to 1 decimal point

Filter: Clean Data

Parameters:
- Standardise numbers for these hashtags = #hxltag
- Format for cleaned numbers (optional) = 0.1f

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=clean&filter-label01=Round+to+1+d.p&clean-num-tags01=%23indicator%2Bvalue%2Bvalue1&clean-number-format01=0.1f&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D0


### Format a decimal as a percent

Filter: Add Column

Parameters:
- Fixed value for new column = {{ #hxltag * 100 }}%

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=add&add-tag01=%23indicator%2Bvalue%2Bvalue3&add-value01=%7B%7B%23indicator%2Bvalue%2Bvalue2+*100%7D%7D%25&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D0

### Fill blanks/empty values with 'No Data'

Filter: Replace Data

Parameters:
- Text or pattern to replace = ^$
- Replacement text = No Data
- Check Use a regular expression pattern checkbox
- Replace only in these columns = #hxltag


Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=replace&replace-pattern01=%5E%24&replace-regex01=on&replace-value01=No+Data&replace-tags01=%23indicator%2Bvalue&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D1595937375
