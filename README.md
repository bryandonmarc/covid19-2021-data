# covid19-2021-data
2021 CSV Data from DOH Data Drop used for Project CoViD v3.0



## Metadata
| File                              | Description                                                                                                                                                      |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`data.csv`](#datacsv)            | Contains cleaned, filtered data from DOH Data Drop for Lagunense                                                                                                 |
| `date.json`                       | Contains the date for the current data in `data.csv`                                                                                                             |
| [`Barangay.json`](#barangayjson)* | JSON file containing the list of City/Municipalities nested as `CityMuniDigit` objects whose attributes corresponds to the Barangays under it as `BarangayDigit` |
| `CityMunicipality.json`*          | JSON file containing the list of City/Municipality names with `CityMuniDigit` as key                                                                             |

**All name values are split into an array of words*



## `data.csv`
| CSV Column | Data                                   | Values                                                                                                                                                                           |
|------------|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| v.0        | Age Group                              | <ul><li>`1`: 'UNKNOWN'</li><li>`2`: '<9'</li><li>`3`: '10-17'</li><li>`4`: '18-39'</li><li>`5`: '40-59'</li><li>`6`: '60-79'</li><li>`7`: '80+'</li></ul>                        |
| v.1        | Sex                                    | <ul><li>`M`: 'Male'</li><li>`F`: 'Female'</li></ul>                                                                                                                              |
| v.2        | Health Status*                         | <ul><li>`R`: 'Recovered'</li><li>`D`: 'Deceased'</li><li>`A`: 'Asymptomatic'</li><li>`m`: 'Mild'</li><li>`M`: 'Moderate'</li><li>`S`: 'Severe'</li><li>`C`: 'Critical'</li></ul> |
| v.3        | Year Publicly Announced as Case        | Year, e.g. `2020`                                                                                                                                                                |
| v.4        | Month Publicly Announced as Case       | Month, e.g. `3`                                                                                                                                                                  |
| v.5        | Day Publicly Announced as Case         | Day, e.g. `14`                                                                                                                                                                   |
| v.6        | Encoded Barangay and Municipality Key^ | Hash value, e.g. `2352`                                                                                                                                                          |

**All Cases listed in `data.csv` are categorized and also count as Confirmed*

*^The Barangay and Municipality can be decoded using the following function below*

```js
const decode = (c) => {
  const a = Math.floor(c / 84).toString()
  const b = (c % 84).toString()
  return { a, b }
}

//e.g. 2352
const { a, b } = decode(2352)
const CityMuniDigit = a
const BarangayDigit = b

const CityMuniPSGC = 'PH0434' + CityMuniDigit.toString().padStart(2, '0') + '000'
const BarangayPSGC =
  'PH0434' +
  CityMuniDigit.toString().padStart(2, '0') +
  BarangayDigit.toString().padStart(3, '0') +
  (BarangayDigit === '000' ? '-UNKNOWN' : '')
  
console.log(CityMuniPSGC) // PH043428000
console.log(BarangayPSGC) // PH043428000-UNKNOWN
```



## `Barangay.json`
For example,
```json
{
  "1": {
    "1": [
      "DEL",
      "CARMEN"
    ],
    "2": [
      "PALMA"
    ],
    "3": [
      "BARANGAY",
      "I",
      "(POB.)"
    ],
  }
}
```
With a `CityMuniDigit` value of `1` and a `BarangayDigit` value of `2`, you get `["PALMA"]`
> If either `CityMuniDigit` or `BarangayDigit` equals to `0` when decoding the key, it means the city/municipality or barangay is `UNKNOWN`








