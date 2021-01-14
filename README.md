# HXL Proxy Transformation Pattern Library

## Filtering

### Filter for latest date

Filter: Select Rows

Notes: Make sure data is formatted to YYYY-MM-DD

Parameters:
- Queries = `#date is max`

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=explode&filter-label01=Pivot+admin+columns+to+values+in+single+column&explode-header-att01=header&explode-value-att01=value&filter02=rename&filter-label02=Rename+column+header&rename-oldtag02=%23affected%2Bactive_cases%2Bheader&rename-newtag02=%23adm1%2Bname&rename-header02=admin1&filter03=rename&filter-label03=Rename+column+header&rename-oldtag03=%23affected%2Bactive_cases%2Bvalue&rename-newtag03=%23affected%2Bactive_cases&rename-header03=Active+cases&filter04=select&filter-label04=Filter+data+to+latest+date+only&select-query04-01=%23date+is+max&tagger-match-all=on&tagger-default-tag=%23affected%2Bactive_cases%2Blabel&tagger-01-header=kasus+aktif&tagger-01-tag=%23date&header-row=1&url=https%3A%2F%2Fdrive.google.com%2Fu%2F2%2Fuc%3Fid%3D11wED0XH5p8OwU36qd92PqfBVacpi-EEW%26export%3Ddownload&sheet=2

---

## Calculated Columns

### Extract just year from date

Notes: Make sure data is formatted to YYYY-MM-DD

Filter: Replace data

Parameters:
- Text or pattern to replace = `(\d{4})-.*`
- Use a regular expression pattern = `checked/true`
- Replacement text = `\1`

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=clean&filter-label01=Clean+data+formats&clean-date-tags01=%23date&filter02=replace&filter-label02=Extract+just+the+year&replace-pattern02=%28%5Cd%7B4%7D%29-.%2A&replace-regex02=on&replace-value02=%5C1&replace-tags02=%23date&filter03=count&filter-label03=Count+rows+by+org%2C+sector+and+year&count-tags03=%23org%2C%23sector%2C%23date&count-type03-01=count&count-header03-01=Count&count-column03-01=%23meta%2Bcount&filter04=select&filter-label04=select+just+2020&select-query04-01=%23date%3D2020&filter05=implode&filter-label05=Pivot+orgs+from+single+column+to+multiple+columns&implode-label-pattern05=%23org&implode-value-pattern05=%23meta%2Bcount&filter06=cut&filter-label06=remove+date+column&cut-exclude-tags06=%23date&filter07=rename&filter-label07=Change+column+header+on+sector&rename-oldtag07=%23sector&rename-newtag07=%23sector&rename-header07=Sector&filter08=sort&filter-label08=Sort+sector+alphabetically&sort-tags08=%23sector&filter09=add&add-tag09=%23indicator%2Bdeployments&add-value09=%7B%7Bsum%28%23meta%2Bcount%29%7D%7D&add-header09=Total&add-before09=on&tagger-match-all=on&tagger-02-header=organization&tagger-02-tag=%23org&tagger-03-header=functional&tagger-03-tag=%23sector&tagger-12-header=start+date&tagger-12-tag=%23date%2Bstart&header-row=1&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F15Y2BzrOHy_8Vh1BvO1FuotowdtFQHOUtedguzLGdhQA%2Fedit%23gid%3D306007431

---

### Calulate difference between today's figure and the previous day's figure

#### Dataset 1

Filters: Select Rows

Parameters:
- Queries = `#date is max`

#### Dataset 2

Notes: Using the same dataset in a new query

Filter: Select Rows

Parameters:
- Queries =  `#date is max`
- Invert query (include rows that don't match any of the queries) = `checked/true`

Filter: Select Rows

Parameters:
- Queries =  `#date is max`
- Invert query (include rows that don't match any of the queries) = `unchecked/false`

Notes: Date is now filtered to previous day

#### Dataset 1

Filter: Merge from another datasets

URL of merge dataset (pull extra data from here) = `url of dataset2`
Shared keys (values that are the same in both datasets) = `#hxltag`
Tags to copy from other dataset = `#hxltag`

Filter: Add column

Parameters:
Tag for new column = `#hxltag+daily_change`
Fixed value for new column =  `{{(#hxltag+current_day - #hxltag+previous_day)/(#hxltag+previous_day)*100}}

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=explode&filter-label01=Pivot+admin+columns+to+values+in+single+column&explode-header-att01=header&explode-value-att01=value&filter02=rename&filter-label02=Rename+column+header&rename-oldtag02=%23affected%2Bactive_cases%2Bheader&rename-newtag02=%23adm1%2Bname&rename-header02=admin1&filter03=rename&filter-label03=Rename+column+header&rename-oldtag03=%23affected%2Bactive_cases%2Bvalue&rename-newtag03=%23affected%2Bactive_cases&rename-header03=Active+cases&filter04=select&filter-label04=Filter+data+to+latest+date+only&select-query04-01=%23date+is+max&filter05=rename&rename-oldtag05=%23affected%2Bactive_cases&rename-newtag05=%23affected%2Bactive_cases%2Bcurrent_day&filter06=merge&filter-label06=Merge+column+of+dataset+filter+to+previous+day&merge-url06=https%3A%2F%2Fproxy.hxlstandard.org%2Fdata.csv%3Fdest%3Ddata_edit%26filter01%3Dexplode%26filter-label01%3DPivot%2Badmin%2Bcolumns%2Bto%2Bvalues%2Bin%2Bsingle%2Bcolumn%26explode-header-att01%3Dheader%26explode-value-att01%3Dvalue%26filter02%3Drename%26filter-label02%3DRename%2Bcolumn%2Bheader%26rename-oldtag02%3D%2523affected%252Bactive_cases%252Bheader%26rename-newtag02%3D%2523adm1%252Bname%26rename-header02%3Dadmin1%26filter03%3Drename%26filter-label03%3DRename%2Bcolumn%2Bheader%26rename-oldtag03%3D%2523affected%252Bactive_cases%252Bvalue%26rename-newtag03%3D%2523affected%252Bactive_cases%26rename-header03%3DActive%2Bcases%26filter04%3Dselect%26filter-label04%3DFilter%2Bdata%2Bto%2Blatest%2Bdate%2Bonly%26select-query04-01%3D%2523date%2Bis%2Bmax%26select-reverse04%3Don%26filter05%3Dselect%26select-query05-01%3D%2523date%2Bis%2Bmax%26filter06%3Drename%26rename-oldtag06%3D%2523affected%252Bactive_cases%26rename-newtag06%3D%2523affected%252Bactive_cases%252Bprevious_day%26rename-header06%3DPrevious%2BDays%2Bcases%26tagger-match-all%3Don%26tagger-default-tag%3D%2523affected%252Bactive_cases%252Blabel%26tagger-01-header%3Dkasus%2Baktif%26tagger-01-tag%3D%2523date%26header-row%3D1%26url%3Dhttps%253A%252F%252Fdrive.google.com%252Fu%252F2%252Fuc%253Fid%253D11wED0XH5p8OwU36qd92PqfBVacpi-EEW%2526export%253Ddownload%26sheet%3D2&merge-keys06=%23adm1%2Bname&merge-tags06=%23affected&filter07=add&filter-label07=Add+a+column+of+the+difference+between+the+two+dates+as+a+percent+of+the+previous+days+figure&add-tag07=%23affected%2Bchange&add-value07=%7B%7B%28%23affected%2Bactive_cases%2Bcurrent_day+-+%23affected%2Bactive_cases%2Bprevious_day%29%2F%28%23affected%2Bactive_cases%2Bprevious_day%29%2A100%7D%7D&add-header07=Change&filter08=clean&clean-num-tags08=%23affected%2Bchange&clean-number-format08=0.1f&tagger-match-all=on&tagger-default-tag=%23affected%2Bactive_cases%2Blabel&tagger-01-header=kasus+aktif&tagger-01-tag=%23date&header-row=1&url=https%3A%2F%2Fdrive.google.com%2Fu%2F2%2Fuc%3Fid%3D11wED0XH5p8OwU36qd92PqfBVacpi-EEW%26export%3Ddownload&sheet=2

---

## Formatting

### Format number to 1 decimal point

Filter: Clean Data

Parameters:
- Standardise numbers for these hashtags = `#hxltag`
- Format for cleaned numbers (optional) = `0.1f`

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=clean&filter-label01=Round+to+1+d.p&clean-num-tags01=%23indicator%2Bvalue%2Bvalue1&clean-number-format01=0.1f&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D0

---

### Format a decimal as a percent

Filter: Add Column

Parameters:
- Fixed value for new column = `{{ #hxltag * 100 }}%`

Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=add&add-tag01=%23indicator%2Bvalue%2Bvalue3&add-value01=%7B%7B%23indicator%2Bvalue%2Bvalue2+*100%7D%7D%25&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D0

---

### Fill blanks/empty values with 'No Data'

Filter: Replace Data

Parameters:
- Text or pattern to replace = `^$`
- Replacement text = `No Data`
- `Check` Use a regular expression pattern checkbox
- Replace only in these columns = `#hxltag`


Example: https://proxy.hxlstandard.org/data/edit?dest=data_edit&filter01=replace&replace-pattern01=%5E%24&replace-regex01=on&replace-value01=No+Data&replace-tags01=%23indicator%2Bvalue&url=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1LhM-VW7sPyumEFocf3NPRRZx1PUJqVwFrN9jOKsI1aI%2Fedit%23gid%3D1595937375
