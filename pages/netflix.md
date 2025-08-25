## Evidence Demo

### Netflix Data Table
```netflix_query_summary
SELECT * FROM netflix_dataset.netflix_all
```
<DataTable data={netflix_query_summary} />


### Shows by Release Year-Month
```shows_by_release_year_month
SELECT
    strftime(strptime(Release_Date, '%B %d, %Y'), '%Y-%m') AS release_year_month,
    Category,
    COUNT(DISTINCT Show_id) as shows
FROM netflix_dataset.netflix_all
GROUP BY strftime(strptime(Release_Date, '%B %d, %Y'), '%Y-%m'), Category
ORDER BY 1 DESC
```
<BarChart
    data={shows_by_release_year_month}
    x=release_year_month
    y=shows
	series=Category
	xAxisTitle="Year Month"
	yAxisTitle="Shows Released"
    title="Shows Released by Category"
/>


<LastRefreshed prefix="Data last updated"/>
