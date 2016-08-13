# SkyWatch API v0.1

## Overview
The FIRST version of the SkyWatch API is now ready for YOU. Through the API you have access to the following climate and atmospheric datasets:
- [AIRS]
- [ACOS]
- [MOPITT]
- [OCO-2]
- [TES]

Through the API access you will be able to search datasets by location and date, and use the results to pull the respective data from our FTP server.

## API Usage
```sh
curl -H "x-api-key: $API_KEY" https://cqh77pglf1.execute-api.us-west-2.amazonaws.com/prod/data/location/$COORDINATES/time/$TIMES
```

Where:

### `$API_KEY`
A personal alphanumeric api key will be provided to you upon request. If you weren’t provided a key, or have any issues using the key provided, please contact dexter@skywatch.co or dexter on Slack.

### `$COORDINATES`
A list of longitude, latitude coordinate pairs as a flat, comma-separated list. A list of two numbers represents a point, four numbers is a square area where the coordinates are the corners, or if there are more than four numbers the coordinates represent a closed polygon, where the first point equals the last point in the list. Because this list represents a number of points, there always has to be an even number of numbers in the list.

Examples:
- Point: `-71.1043443253,-42.3150676016`
- Square: `71.1043443253471,-42.3150676015829,71.1043443253471,-42.3150676015829,71.1043443253471,42.3150676015829,-71.1043443253471,42.3150676015829`
- Polygon: `71.1043443253471,-42.3150676015829,71.1043443253471,-42.3150676015829,71.10434432 53471,42.3150676015829,-71.1043443253471,42.3150676015829,-71.1043443253471,-42.31 50676015829`

### `$TIMES`
One or two UTC timestamps in ISO 8601 format (yyyy-mm-ddThh:mm:ss.sssss+|-zzzz). Partial dates can also be specified: 2009, 2009-12, 2009-12-25, 2009-12-25T13:25:00.0000+0000. If no time is specified, midnight UTC on the day in question is assumed.
If only one timestamp is passed in, a one day range is assumed. For example, if 2009-12-25 is specified, the search takes place as if 2009-12-25,2009-12-26 was specified.

## Example API Call
```sh
curl -H "x-api-key: 5E5bkcTPB62bPwJXvqGTR9fwg5rBhHQY8iayxhSk" https://cqh77pglf1.execute-api.us-west-2.amazonaws.com/prod/data/location/-71.1043443253471,-42.3150676 015829/time/2009-12-25
```
If you get a status 200, you will receive a JSON list of CO2 satellite data files and their sizes that match the criteria.

Example:

```json
[
  {
    "size": 159422423,
    "download_path": "\/mnt\/skywatch-co2\/MOPITT\/2009.12.24\/MOP01-20091224-L1V3.42.0.he5"
  },
  {
    "size": 159084071,
    "download_path": "\/mnt\/skywatch-co2\/MOPITT\/2009.12.25\/MOP01-20091225-L1V3.42.0.he5"
  },
  {
    "size": 2550115,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_11_Production_v161160_L2s30504_r01_PolB_160129193422. h5"
  },
  {
    "size": 2585404,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_17_Production_v161160_L2s30504_r01_PolB_160129193432. h5"
  },
  {
    "size": 3404578,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_20_Production_v161160_L2s30504_r01_PolB_160129193447. h5"
  },
  {
    "size": 3418359,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_14_Production_v161160_L2s30504_r01_PolB_160129193427. h5"
  },
  {
    "size": 2581921,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_32_Production_v161160_L2s30504_r01_PolB_160129193512. h5"
  },
  {
    "size": 3390192,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_35_Production_v161160_L2s30504_r01_PolB_160129193518. h5"
  },
  {
    "size": 2749116,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_38_Production_v161160_L2s30504_r01_PolB_160129193523. h5"
  },
  {
    "size": 2493449,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_23_Production_v161160_L2s30504_r01_PolB_160129193452. h5"
  },
  {
    "size": 1308855,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_29_Production_v161160_L2s30504_r01_PolB_160129193502. h5"
  },
  {
    "size": 1151860,
    "download_path": "\/mnt\/skywatch-co2\/ACOS\/acos_L2s_091225_26_Production_v161160_L2s30504_r01_PolB_160129193457. h5"
  }
]
```

Accessing the Data
Once you have the list of returned data sets based on the API call, you can now access the paths specified in the JSON responses through your preferred FTP client (e.g. Filezilla, CuteFTP, etc.)

FTP Login Details:
- Host: 52.42.247.111
- Username: skywatch-ftp
- Password: SkyWatch151FTP

You may now download the file listed in the path of a response (e.g. `/mnt/skywatch-co2/MOPITT/2009.12.24/MOP01-20091224-L1V3.42.0.he5`).

## Troubleshooting
Error-handling is not great - returning error messages through the API is still something that needs to be implemented.
For any issues or questions, please contact  dexter@skywatch.co  or  dexter on Slack.

[AIRS]: http://www.skywatch.co/airs
[ACOS]: http://disc.sci.gsfc.nasa.gov/acdisc/documentation/ACOS.html
[MOPITT]: http://www.skywatch.co/mopitt
[OCO-2]: http://www.skywatch.co/oco2
[TES]: http://www.skywatch.co/tes
