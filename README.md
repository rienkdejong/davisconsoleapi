# davisconsoleapi
Driver for DAVIS Console 6313
```
Version: 
	0.1 initial release
	0.2 added dewpoint1, heatindex1, wetbulb1 for second Vantage/VUE
	0.3 added DAvis Airlink sensor, the Airlink health data are not stored in the database!
    		if Signal Strenght of the Airlink should be stored in the database, you can extend  the database:
       		V4: 'sudo echo "y" | wee_database --config=/etc/weewx/weewx.conf --add-column=rssiA --type=REAL'
                V5: 'sudo weectl database add-column rssiA --type=REAL -y'
        0.42 added windGustSpeed10 and windGustSpeed10_2 to database schema
             or manually add to database: V5: 'sudo weectl database add-column windGustSpeed10 --type=REAL -y'
             changed default_unit_format_dict["microgram_per_meter_cubed"] to "%.1f" (default was "%.0f")
             The "polling_interval" can now be changed in the weewx.conf. Default value = 300 seconds, minimum value = 60 seconds,
             but a corresponding subscription to DAVIS is required
        0.43 added sleep(10) in main packet loop -> rienkdejong

```
# weewx-davisconsoleapi
Collect and display station information from the new Davis Weatherlink Console 6313 API

Weewx driver that pulls sensor information from Davis Instruments Weatherlink Console 6313. 
I made this extension for users like me who have the new DAVIS Weatherlink Console 6313. 

The code makes one API calls each 300 sec (5 min) = archive period = default or what set in weewx.conf (Minimum 60 seconds): 
to the "current" v2 API. 

The data are stored in their own database, since most of the fields don't exist within the default weewx database. 

Please note: This driver don't pull from the "Historic Data" API2, which 
requires a paid monthly subscription from Davis to use (starting at about $4/month = Pro, Pro+ = about $9/month). 

See here: https://weatherlink.github.io/v2-api/data-permissions and
          https://www.davisinstruments.com/pages/weatherlink-cloud

# You should know:
- only from a Vantage VUE you can get data for 
	'TX Battery Volt, Supercap Volt and Solar Panel Volt'

If the current console data are not more updated (values flatlined), you need to do this ( is from support@davisinstruments.com )
- REBOOT the unit (= Console):
  Unplug the USB cable, and then insert a paper clip in to the hole labeled P (indicated as Power Off Access) 
  for 10 seconds pressing the button inside.
  -> is 'Power off' the Console -> Screen is than black!


## Update from davisconsolehealthapi
If you would like to use the data from davisconsolehealthapi: This driver includes the function (Health Data) of the davisconsolehealthapi extension.
Copy the database file from the davisconsolehealthapi driver: 
'davisconsolehealthapi.sdb'
to the here used database:
'davisconsole.sdb'

With the file 'add_console.sh' you can extend the database to the here used database schema

Maybe you need to modify in this files this settings:
'--config=/etc/weewx/weewx.conf' 
to your prefered setting


## Data
Right now, the driver records all the information from the Davis Console API2,
whereas the 

current data are each 300 sec (5 min) or what set in weewx.conf updated 

and the health data in current packet 
health data are each 900 sec (15 min) updated
so 2 times or more repeated
```
From DAVIS API2
Example bar data:
  "bar_absolute":27.41
  "bar_sea_level":30.248
  "bar_offset":0
  "bar_trend":-0.012
  "ts":1695724800

Example inside data:
  "temp_in":74.9
  "wet_bulb_in":61.9
  "heat_index_in":74.7
  "dew_point_in":54.1
  "wbgt_in":null
  "ts":1695724800
  "hum_in":48.4

Example ISS data:
  "rx_state":0
  "wind_speed_hi_last_2_min":4
  "hum":64.6
  "freq_index":0
  "last_packet_received_timestamp":1695724797
  "rainfall_day_clicks":0
  "packets_missed_day":244
  "wind_dir_at_hi_speed_last_10_min":162
  "wind_chill":69.3
  "et_month":2.903
  "packets_received_streak_hi_day":451
  "rain_rate_hi_last_15_min_clicks":0
  "thw_index":69.2
  "packets_received_streak":17
  "freq_error_current":-1
  "wind_dir_scalar_avg_last_10_min":162
  "solar_energy_day":168.47000122070312
  "spars_rpm":null
  "wind_run_day":30.420000076293945
  "rain_size":2
  "uv_index":4.2
  "rain_storm_current_in":null
  "rainfall_day_mm":0
  "wind_speed_last":0
  "resyncs_day":0
  "rainfall_last_60_min_clicks":0
  "wet_bulb":61.3
  "rain_storm_current_mm":null
  "rainfall_day_in":0
  "hdd_day":4.7
  "wind_speed_avg_last_10_min":1.33
  "wind_dir_at_hi_speed_last_2_min":161
  "supercap_volt":null
  "solar_panel_volt":null
  "wind_dir_last":168
  "rainfall_month_clicks":120
  "rain_storm_last_clicks":62
  "tx_id":1
  "rain_storm_last_start_at":1695383862
  "packets_received_day":17230
  "rainfall_last_15_min_in":0
  "rain_rate_hi_clicks":0
  "rainfall_last_15_min_mm":0
  "dew_point":56.9
  "rain_rate_hi_in":0
  "rain_rate_hi_mm":0
  "rainfall_year_clicks":4715
  "rain_storm_last_end_at":1695542739
  "wind_dir_scalar_avg_last_2_min":164
  "reception_day":97
  "wbgt":null
  "heat_index":69.2
  "rainfall_last_24_hr_in":0
  "rainfall_last_60_min_mm":0
  "rain_storm_current_clicks":null
  "trans_battery_flag":0
  "rainfall_last_60_min_in":0
  "rainfall_last_24_hr_mm":0
  "crc_errors_day":99
  "rainfall_year_in":37.125984
  "rssi_last":-84
  "wind_speed_hi_last_10_min":5
  "rainfall_last_15_min_clicks":0
  "cdd_day":0.193
  "rainfall_year_mm":943
  "freq_error_total":100
  "wind_dir_scalar_avg_last_1_min":169
  "uv_dose_day":4.329999923706055
  "temp":69.3
  "trans_battery_volt":null
  "et_day":0.041
  "rainfall_month_in":0.9448819
  "wind_speed_avg_last_2_min":1.26
  "rain_storm_current_start_at":null
  "spars_volt":null
  "solar_rad":599
  "rain_storm_last_mm":12.4
  "wind_speed_avg_last_1_min":1.6
  "thsw_index":75.2
  "rain_rate_last_mm":0
  "rain_rate_last_clicks":0
  "rainfall_last_24_hr_clicks":0
  "rain_storm_last_in":0.48818898
  "et_year":22.1
  "packets_missed_streak_hi_day":2
  "rain_rate_last_in":0
  "rain_rate_hi_last_15_min_mm":0
  "rain_rate_hi_last_15_min_in":0
  "rainfall_month_mm":24
  "ts":1695724800
  "packets_missed_streak":0

Example health data
  "battery_voltage":4226
  "wifi_rssi":-56
  "console_radio_version":"10.3.2.90"
  "console_api_level":28
  "queue_kilobytes":4
  "free_mem":699453
  "system_free_space":747118
  "charger_plugged":1
  "battery_percent":100
  "local_api_queries":null
  "health_version":1
  "link_uptime":31130
  "rx_kilobytes":20465563
  "console_sw_version":"1.2.13"
  "connection_uptime":31113
  "os_uptime":1187518
  "battery_condition":2
  "internal_free_space":2215120
  "battery_current":0.003
  "battery_status":5
  "database_kilobytes":108347
  "battery_cycle_count":1
  "console_os_version":"1.2.11"
  "bootloader_version":2
  "clock_source":2
  "app_uptime":1186355
  "tx_kilobytes":181662
  "battery_temp":33                  it's not known if this is °C or what else (seems to be °C)
  "ts":1695724200
```
## Skin 
In this installation there are also skin files (based on the Season skin) for 
    `console`
    `healthc`
with englisch and german language support files 

## Installation
```
Install the extension:
    sudo wee_extension --install=weewx-davisconsoleapi.zip   #V4
    sudo weectl extension install weewx-davisconsoleapi.zip  #V5
or
    sudo wee_extension --install=weewx-davisconsoleapi.zip --config=/etc/weewx1/weewx.conf #V4
    sudo weectl extension install weewx-davisconsoleapi.zip --config=/etc/weewx/weewx.conf #V5

    After the installation, the weewx.conf file must be adapted:
  -   [Station] station_type = DavisConsoleAPI 
  -   [DataBindings] [[wx_binding]] database = davisconsoleapi_sqlite
  -   [DataBindings] [[wx_binding]] schema = schemas.wview_davisconsoleapi.schema
  -   [StdReport] [[SeasonsReport]] enable = false
  -   [DavisConsoleAPI] station_id = %your station_id%
  -   [DavisConsoleAPI] api_key = %your api2_key%
  -   [DavisConsoleAPI] api_secret = %your api2_secret%
  -   [DavisConsoleAPI] txid_iss = 1 	and the other stations, if available
  -   [DavisConsoleAPI] airlink = 0 	set to 1 if also a Airlink Senosr is available
```
## Configuration
By default, the installer installs `davisconsoleapi` as a driver. 
It runs during every archive interval and inserts data into the default SQL database.

Example/Settings in the weewx.conf
#
```
[Station]
    [[Services]]
        #data_services = user.davisconsolehealthapi.DavisConsoleHealthAPI,
this driver includes the console health data, so a extra data service isn't anymore necessary

[StdReport]
    [[DavisConsole]]
        HTML_ROOT = /var/www/html/weewx
        lang = en
        enable = true
        skin = console

    [[DavisHealthConsole]]
        HTML_ROOT = /var/www/html/weewx/healthc
        lang = en
        enable = true
        skin = healthc

[DataBindings]
    [[wx_binding]]
        database = davisconsoleapi_sqlite
        table_name = archive
        manager = weewx.manager.DaySummaryManager
        schema = schemas.wview_davisconsoleapi.schema

[Databases]
    [[davisconsoleapi_sqlite]]
        database_type = SQLite
        database_name = davisconsole.sdb

[DavisConsoleAPI]
    driver = user.davisconsoleapi
    station_id = 1?????
    polling_interval = 300 # minimum 60 sec 
    api_key = ????????????????????????????????
    api_secret = ????????????????????????????????
    txid_iss = 1
    txid_iss2 = None
    txid_leaf = None
    txid_soil = None
    txid_leaf_soil = None
    txid_extra1 = None      # yet not supported by Davis Console
    txid_extra2 = None      # yet not supported by Davis Console
    txid_extra3 = None      # yet not supported by Davis Console
    txid_extra4 = None      # yet not supported by Davis Console
    txid_rain = None        # seems that yet not supported by Davis Console
    txid_wind = None        # supported ?
    airlink = 0             # is Airlink sensor available? # new in v0.3
    packet_log = 0

#packet_log = -1 -> only current rain data
#packet_log = 0 -> none logging
#packet_log = 1 -> check new Archive and Rain
#packet_log = 2 -> current console (bar/temp/hum) packets
#packet_log = 3 -> current ISS/VuE packets
#packet_log = 4 -> current leaf soil packets
#packet_log = 5 -> current extra_data1..4 packets
#packet_log = 6 -> current rain and/or wind packets
#packet_log = 7 -> current iss2 or vue2 packets
#packet_log = 8 -> current health packets
#packet_log = 9 -> all current packets

## settings for 'user.sunrainduration.SunshineDuration' calculates sunshine duratation and rain duration
#more information about this extension can you find in 'sunrainduration.py'

#the installer installs this extensions too, but not enabeld, to enable it:
  [Station]
    [[Services]]
       process_services = ...,user.sunrainduration.SunshineDuration,

  [RadiationDays]			#this are the default settings
    min_sunshine = 120
    sunshine_coeff = 0.95
    sunshine_min = 18
    sunshine_loop = 1
    rainDur_loop = 0
    sunshine_log = 0
    rainDur_log = 0

    rain2 = 1			# second Vantage/VUE available, so calculate rain duration
    sunshine2 = 0			# second Vantage available, so calculate sunshine duration    
    sunshine2_loop = 1
    rainDur2_loop = 0
    sunshine2_log = 0
    rainDur2_log = 0

[Accumulator]
   [[consoleRadioVersionC]]
        accumulator = firstlast
        extractor = last
        
   [[consoleSwVersionC]]
        accumulator = firstlast
        extractor = last
        
   [[consoleOsVersionC]]
        accumulator = firstlast
        extractor = last
```
### API keys
Once installed, you need to add your Davis WeatherLink Cloud API key, station ID, and secret. 
To obtain an API key and secret, go to your WeatherLink Cloud account page and look for a button marked "Generate v2 Token." 
Once you complete the process, enter your key and secret where indicated in weewx.conf.

### Station ID
Davis doesn't make it easy to make API calls, and you have to make an API call to get your station ID. 
To help the process along, I adapted one of Davis' example Python scripts to make an API call that shows your station ID. 
To use it, look for the file `davis_api_toolc.py` in the zip file you downloaded. 
Open it in a text editor and type in your API key/secret where indicated. 
Save the file and run it like so:

`python3 davis_api_toolc.py`

It should return 3 URLs. Open that in a browser (don't delay, the timestamp is encoded in that URL and the Davis API will reject the call 
if you wait too long to make the call) and you'll get back a string of text. Your station ID will be near the beginning. 
Enter that number into weewx.conf and you should be good to go.
       v2 API URL: Stations
       v2 API URL: Current
       v2 API URL: Historic (only availabe with paid subscription from Davis) so here not supported!

## Database schema
This driver uses it's own database schema
`wview_davisconsoleapi.py`

## Usage
Once you enable the driver and get it running, you won't notice anything different. 

### Own skin console and healthc
The files for this new skin (additional to the Seasons skin) are found under 
`skins/console`
`skins/healthc`

The necessary file are there stored during the installation.
Also the stanza in the weewx.conf


Settings for Davis Console skins:
```
        [StdReport]
           [[DavisConsole]]
              #HTML_ROOT = /var/www/html/weewx/console
              lang = en			# or lang = de
              enable = true
              skin = console

          [[DavisHealthConsole]]
             HTML_ROOT = /var/www/html/weewx/healthc
             enable = true
             lang = en			# or lang = de
             skin = healthc 

In skin.conf (example):
        [[[daysignalC]]]
            title = Signal Strength
            yscale = -100.0, -30.0, 10
            [[[[rssiC]]]]
```
This should give you a result like this:

Take a look at the example files to see how that's been done so you can adapt it for your own purposes.

https://www.pc-wetterstation.de/wetter/weewx7/index.html

https://www.pc-wetterstation.de/wetter/weewx7/healthc/index.html

