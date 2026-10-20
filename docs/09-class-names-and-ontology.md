# Class names and ontology

The className field is used to indicate to consumers of FD data that points are of specific physical types. For example, some analogue signals could be flows which would indicate to the SA that specific display characteristics are associated like a flow meter graphic.

Users can enter any text into this field, generally specific to the type(s) of SA they have.

Users can separate parts of the class name text with '**.**' Or '**/**' for example to indicate a structure.

## Fixed class names

The className of some items can use some common definitions listed here. This is to ensure consistency for device management. By allowing the Configuration file to define these items, signals such as Battery can be assigned to different device terminals, which is sometimes the case for identical FDs. The className field is used so that the actual point name can be user-configured.

These classNames are prefixed by '**L.**' to indicate that this document defines them.

These items are optional. FDs do not have to include a Battery status, but the use of these is recommended.

The className field structure is not defined, allowing these items to be structured as the application requires.

Reporting functionality of the points is as specified in the Configuration.

| \"className\" field | Type | Purpose | Specifics |
|---------------------------|--------------------|-----------------------------------------|-------------|
| L.FD.Battery | Binary input | Battery status | State 1 is healthy |
| L.FD.Mains | Binary input | Main power input | State 1 is healthy |
| L.FD.Signal | Analogue input | Generic common signal indicating communications signal strength | Top of configured range is highest strength |
| L.FD.IMSI | String | IMSI of current cellular modem |  |
| L.FD.IMEI | String | IMEI of current cellular modem |  |
| L.FD.CCID | String | CCID of current cellular modem |  |
| L.FD.Serial | String | Serial number of the field device |  |
| L.Loc.Latitude | Analogue input | FD location | WGS84 Latitude |
| L.Loc.Longitude | Analogue input | FD location | WGS84 Longitude |
| L.Loc.Accuracy | Analogue input | FD Location accuracy | m |
| L.Loc.Altitude | Analogue input | FD Altitude above sea level | m |
| L.Loc.Direction | Analogue input | FD Direction | degrees |
| L.Loc.Speed | Analogue input | FD Speed | m/s |
| L.FD.Batt.Main.Volt | Analogue Input | "Main" FD battery voltage | Volts |
| L.FD.Batt.Main.Charge | Analogue Input | Main battery capacity remaining | \% |
| L.FD.Batt.Sec.Volt | Analogue input | Secondary battery voltage | Volts |
| L.FD.Batt.Sec.Charge | Analogue Input | Secondary battery capacity remaining | \% |
| L.FD.Temp.Int | Analogue Input | FD internal temperature | deg.C |
| L.FD.Temp.Int.Min | Analogue input | Minimum Internal Temperature | deg.C |
| L.FD.Temp.Int.Max | Analogue input | Maximum Internal Temperature | deg.C |
| L.FD.Temp.Int.Mean | Analogue input | Mean Internal Temperature | deg.C |
| L.FD.Temp.Modem | Analogue Input | Modem temperature | deg.C |
| L.FD.Temp.Ext | Analogue Input | FD external temperature | deg.C |
| L.FD.Temp.Ext.Min | Analogue input | Minimum external Temperature | deg.C |
| L.FD.Temp.Ext.Max | Analogue input | Maximum external Temperature | deg.C |
| L.FD.Temp.Ext.Mean | Analogue input | Mean external Temperature | deg.C |
| L.FD.Humidity | Analogue input | FD Humidity | \% |
| L.FD.Humidity.Min | Analogue input | FD Maximum Humidity | \% |
| L.FD.Humidity.Max | Analogue input | FD Miminum Humidity | \% |
| L.FD.Humidity.Mean | Analogue input | FD Mean Humidity | \% |
| L.FD.WAN.Tech | String | WAN technology |  |
| L.FD.WAN.MCC | String | Mobile Country Code | 0-999 |
| L.FD.WAN.MNC | String | Mobile Network Code | 0-999 |
| L.FD.WAN.LAC | String | Location Area Code (or Tracking Area Code) |  |
| L.FD.WAN.TAC | String | Tracking Area Code |  |
| L.FD.WAN.BSIC | String | Base Station Identity Code (2G) |  |
| L.FD.WAN.PSC | String | Primary Scrambling Code (3G) |  |
| L.FD.WAN.Channel | String | ARFCN (2G), UARFCN (3G) or EARFCN (4G) |  |
| L.FD.WAN.Signal | Analogue Input | Signal Strength | dBm |

: Fixed class names
