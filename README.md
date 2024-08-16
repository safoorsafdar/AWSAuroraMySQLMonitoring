# AWS Aurora MySQL IOPs Monitoring

## Baseline I/O Rate
![screenshot](baseline_io_rate.jpg)

```
{
    "metrics": [
        [ { "expression": "m1 + m2", "label": "Total_IOPs", "id": "e1", "region": "us-west-2" } ],
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "id": "m1", "region": "us-west-2" } ],
        [ ".", "VolumeWriteIOPs", ".", ".", { "id": "m2", "region": "us-west-2" } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "region": "us-west-2",
    "title": "Baseline I/O Rate",
    "legend": {
        "position": "right"
    },
    "stat": "Average",
    "period": 60
}
```


## Peak I/O Rate

```
{
    "metrics": [
        [ { "expression": "m1 + m2", "label": "Total_IOPs", "id": "e1", "region": "us-west-2" } ],
        [ { "expression": "MAX([e1])", "label": "MAX_IOPs", "id": "e2", "region": "us-west-2" } ],
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "id": "m1", "region": "us-west-2" } ],
        [ ".", "VolumeWriteIOPs", ".", ".", { "id": "m2", "region": "us-west-2" } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "region": "us-west-2",
    "stat": "Average",
    "period": 60,
    "title": "Peak I/O Rate",
    "legend": {
        "position": "right"
    }
}
```


## Total Duration of Peak I/O Activity

```
{
    "metrics": [
        [ { "expression": "m1 + m2", "label": "Total IOPS", "id": "e1", "region": "us-west-2", "yAxis": "left", "period": 300 } ],
        [ { "expression": "IF(e1 > 150000, 1, 0)", "label": "Peak Threshold Exceeded", "id": "e2", "region": "us-west-2", "yAxis": "left", "period": 300 } ],
        [ { "expression": "SUM([e2])", "label": "Peak Activity Duration (per 5 min)", "id": "e3", "region": "us-west-2", "yAxis": "right", "period": 300 } ],
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "id": "m1", "region": "us-west-2", "yAxis": "left" } ],
        [ ".", "VolumeWriteIOPs", ".", ".", { "id": "m2", "region": "us-west-2", "yAxis": "left" } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "region": "us-west-2",
    "stat": "Average",
    "period": 300,
    "title": "Total Duration of Peak I/O Activity",
    "singleValueFullPrecision": false,
    "yAxis": {
        "left": {
            "label": "IOPS",
            "showUnits": true
        },
        "right": {
            "label": "Duration (per 5 min)",
            "showUnits": true
        }
    },
    "legend": {
        "position": "right"
    },
    "annotations": {
        "horizontal": [
            {
                "label": "peak_threshold",
                "value": 150000
            }
        ]
    },
    "setPeriodToTimeRange": false,
    "sparkline": true,
    "trend": true
}
```



## Baseline IO Rate per Hour

```
{
    "metrics": [
        [ { "expression": "AVG([m1, m2])", "label": "Baseline IO Rate", "id": "e1", "region": "us-west-2", "period": 3600 } ],
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "period": 60, "id": "m1", "region": "us-west-2" } ],
        [ ".", "VolumeWriteIOPs", ".", ".", { "period": 60, "id": "m2", "region": "us-west-2" } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "title": "Baseline IO Rate per Hour",
    "stat": "Average",
    "period": 3600,
    "region": "us-west-2"
}
```

## Peak IO Rate per Hour

```
{
    "metrics": [
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "id": "m1", "region": "us-west-2", "period": 3600 } ],
        [ { "expression": "MAX([m1, m2])", "label": "Peak IO Rate", "id": "e2", "region": "us-west-2", "period": 3600 } ],
        [ "AWS/RDS", "VolumeWriteIOPs", "DBClusterIdentifier", "db-production", { "period": 60, "id": "m2", "region": "us-west-2" } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "region": "us-west-2",
    "period": 300,
    "title": "Peak IO Rate per Hour"
}
```

## Aurora MySQL IO Monitoring

```
{
    "metrics": [
        [ "AWS/RDS", "VolumeReadIOPs", "DBClusterIdentifier", "db-production", { "id": "m1", "region": "us-west-2", "period": 3600 } ],
        [ ".", "VolumeWriteIOPs", ".", ".", { "id": "m2", "region": "us-west-2", "period": 3600 } ],
        [ { "expression": "AVG([m1, m2])", "label": "Baseline IO Rate", "id": "e1", "region": "us-west-2", "period": 3600 } ],
        [ { "expression": "MAX([m1, m2])", "label": "Peak IO Rate", "id": "e2", "region": "us-west-2", "period": 3600 } ],
        [ { "expression": "SUM([e2])*3600/(30*24*60*60)", "label": "Duration of Peak Activity (hours per month)", "id": "e3", "region": "us-west-2", "period": 2592000 } ],
        [ { "expression": "SUM([e2]*3600)/86400", "label": "Duration of Peak Activity (hours per day)", "id": "e4", "region": "us-west-2", "period": 86400 } ]
    ],
    "view": "timeSeries",
    "stacked": false,
    "region": "us-west-2",
    "stat": "Average",
    "period": 300,
    "title": "Aurora MySQL IO Monitoring _BETA",
    "singleValueFullPrecision": false,
    "yAxis": {
        "left": {
            "label": "IOPS",
            "showUnits": true
        },
        "right": {
            "label": "Duration (hours)",
            "showUnits": true
        }
    },
    "legend": {
        "position": "right"
    }
}
```
