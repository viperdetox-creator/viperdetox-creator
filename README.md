# Asher S Jacob

I am learning data engineering, SQL and data quality validation through practical projects.

## I caught a wrong number

The published IPL dataset reported Shubman Gill's 2026 season total as 836 runs, but the original data showed 732 runs — the published total was 104 runs too high. The overstatement came from duplicated ball records in one match: every one of the 58 balls Gill faced on 29 May 2026 (match 1535464) appeared twice in the published file, so his 104 runs from that match were counted twice.

| Batter | Published | True |
|---|---:|---:|
| Shubman Gill | 836 | 732 |
| V Suryavanshi | 776 | 776 |
| B Sai Sudharsan | 722 | 722 |
| V Kohli | 675 | 675 |
| H Klaasen | 624 | 624 |

### How I found it

1. Compared the original season's top ten scorers with the published dataset's top ten.
2. Compared Shubman Gill's runs and row counts match by match in both datasets to locate the discrepancy.
3. Grouped his published rows by innings, over and ball in match 1535464 to identify the 58 repeated ball records.

```sql
-- Which balls faced by Shubman Gill in match 1535464 appear more than once in the published dataset?
SELECT
    innings,
    over_no,
    ball_no,
    COUNT(*) AS times
FROM published
WHERE batter = 'Shubman Gill'
  AND match_id = 1535464
GROUP BY
    innings,
    over_no,
    ball_no
ORDER BY times DESC, innings, over_no, ball_no;
```

Data: Cricsheet (cricsheet.org), Open Data Commons Attribution License.
