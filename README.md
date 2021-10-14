# Accesing INTEGRAL SPI-ACS Data 

The data can be access at:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl

For a quick preview of a plot:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs-plot.pl?requeststring=2020-04-28T14:34:33+10&submit=Submit

```sh
$ curl 'https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2020-03-30T09%3A51%3A05+10&generate=ipnlc&submit=Submit'
```

## Latency and availablility

All SPI-ACS data is public. The service provides the data as soon as it is decoded from the telemetry into the near-realtime [Science Window]() data.

## In case of issues

Consult [status page](http://status.reproducible.online/) for any announcements.
Contact me (the contact information should be shown on the page when the request can not be completed).

## List of known triggers

https://www.isdc.unige.ch/integral/ibas/cgi-bin/ibas_acs_web.cgi

## Other ways to access the data

See the platform to access various INTEGRAL data:

https://www.astro.unige.ch/cdci/astrooda_/

## Additional data

INTEGRAL ephemeries can be accessed here:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2021-10-14T19%3A00%3A00&generate=ephs&submit=Submit

Note that it is also possible to request predicted ephemeries, in the future. In this case, predicted orbit is used. Note that predicted attitude is also used for very recent data (hours before).

# Extra Metadata

|  | |
| --- | :-- |
| AKA | https://gitlab.unige.ch/Volodymyr.Savchenko/integral-spi-acs-data | 
| AKA | https://gitlab.astro.unige.ch/savchenk/integral-spi-acs-data | 

