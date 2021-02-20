# Ham Clock Hacks.

This are some hacks that may help you get a feature you want. They are based on an examination of the source code. I will note if I've tried them or not.

## Change Weather Source - I have not tried this - Version 2.58

The source of the weather seems to be entirly determined in `wx.cpp`.

Line 7 The server and initial path:
```
static const char wx_base[] = "/ham/HamClock/wx.pl";
```

Line 27-29 The veriables:
```
snprintf (line, sizeof(line), _FX("%s?is_de=%d&lat=%g&lng=%g"), wx_base, is_de, ll.lat_d, ll.lng_d);
	Serial.println (line);
        httpGET (wx_client, svr_host, line);
```

* `wx_base` The server and initial path, set in line 7.
* `is_de` A boolean value, true if the request of for the local station DE, false otherwise.
* `ll` A structure with latitude and longitude of the station.
* `wx_client` The WiFi Client structure.
* `svr_host` The server host name. Defined in `HamClock.h` line 186 and set in `ESPHamClock.ino` it `clearskyinstitute.com`.
* `line` The bulk of the request URL.

The returned data (for my location) is:
```
city=La Pche
temperature_c=-4.44
humidity_percent=61
wind_speed_mps=2.24
wind_dir_name=N
clouds=overcast clouds
conditions=overcast clouds
attribution=openweathermap.org
```

If your station provides the same data format changing line 26-29 to the following may be all you need to do:
```
        // query web page
if (is_de) {
  snprintf (line, sizeof(line), _FX("%s"), "--The full path part of the URL your station needs.");  // Or code to that effect.
  Serial.println (line);  // This looks like logging.
        httpGET (wx_client, "example.com", line);  // Use the host name of your server
} else {
	snprintf (line, sizeof(line), _FX("%s?is_de=%d&lat=%g&lng=%g"), wx_base, is_de, ll.lat_d, ll.lng_d);
	Serial.println (line);
        httpGET (wx_client, svr_host, line);
}
```

If your format is different you will have to edit liens 48-83 to decode. Or if you have a web server and the skills you could write a transcoder.

