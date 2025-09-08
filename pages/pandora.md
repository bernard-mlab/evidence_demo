---
title: Pandora Core Countries Demo
---

# Countries Data Dashboard

Filter and explore country data from our Pandora core countries database.

```sql countries_list
SELECT DISTINCT
    name
FROM
    bigquery.core_countries
WHERE
    name IS NOT NULL
ORDER BY
    name;
```

```sql regions_list
SELECT DISTINCT
    region
FROM
    bigquery.core_countries
WHERE
    region IS NOT NULL
ORDER BY
    region;
```

```sql companies_list
SELECT DISTINCT
    company_name
FROM
    bigquery.core_countries
WHERE
    company_name IS NOT NULL
ORDER BY
    company_name;
```

```sql filtered_countries
SELECT
    global_entity_id,
    country_code_iso,
    country_code_iso3,
    name,
    region,
    currency_code_iso,
    timezone,
    company_name,
    management_entity,
    braze_tracking_code
FROM
    bigquery.core_countries
WHERE
    1=1
    AND name IN ${inputs.country_filter.value}
    AND region IN ${inputs.region_filter.value}
    AND company_name IN ${inputs.company_filter.value}
ORDER BY
    name;
```

```sql region_summary
SELECT
    region,
    COUNT(*) as country_count,
    COUNT(DISTINCT company_name) as brand_count,
    STRING_AGG(DISTINCT currency_code_iso, ', ') as currencies,
    STRING_AGG(DISTINCT management_entity, ', ') as management_entities
FROM
    bigquery.core_countries
WHERE
    1=1
    AND name IN ${inputs.country_filter.value}
    AND region IN ${inputs.region_filter.value}
    AND company_name IN ${inputs.company_filter.value}
GROUP BY
    region
ORDER BY
    country_count DESC;
```

```sql company_summary
SELECT
    company_name,
    COUNT(*) as country_count,
    COUNT(DISTINCT region) as region_count,
    STRING_AGG(DISTINCT currency_code_iso, ', ') as currencies_supported
FROM
    bigquery.core_countries
WHERE
    1=1
    AND name IN ${inputs.country_filter.value}
    AND region IN ${inputs.region_filter.value}
    AND company_name IN ${inputs.company_filter.value}
GROUP BY
    company_name
ORDER BY
    country_count DESC;
```

## Global Filters

<Dropdown
    name="country_filter"
    data={countries_list}
    value="name"
    title="Select Countries"
    multiple=true
    selectAllByDefault=true
/>

<Dropdown
    name="region_filter"
    data={regions_list}
    value="region"
    title="Select Regions"
    multiple=true
    selectAllByDefault=true
/>

<Dropdown
    name="company_filter"
    data={companies_list}
    value="company_name"
    title="Select Brands"
    multiple=true
    selectAllByDefault=true
/>

## Summary Metrics

**Total Countries:** {filtered_countries.length}
**Regions Represented:** {region_summary.length}
**Brands Operating:** {company_summary.length}

## Regional Overview

<DataTable
    data={region_summary}
    search=true
    title="Countries and Brands by Region"
/>

## Brand Distribution

<BarChart
    data={company_summary}
    x="company_name"
    y="country_count"
    title="Number of Countries by Brand"
/>

## Company Summary

<DataTable
    data={company_summary}
    search=true
    title="Brand Operations Summary"
/>

## Detailed Country Data

<DataTable
    data={filtered_countries}
    search=true
    rows=20
    title="Complete Country Information"
/>

## Key Insights

- **Global Coverage**: Showing data for **{filtered_countries.length}** countries across **{region_summary.length}** regions
- **Brand Diversity**: **{company_summary.length}** different Delivery Hero brands operating across selected markets
- **Management Structure**: Multiple management entities coordinating operations across different geographical areas
- **Currency Support**: Various local currencies supported to facilitate local market operations

## Export Options

You can use the search functionality in each table to find specific countries, regions, or brands. All tables automatically update when you change the filter selections above.
