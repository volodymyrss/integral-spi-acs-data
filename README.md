[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.7472097.svg)](https://doi.org/10.5281/zenodo.7472097)

# Accesing INTEGRAL SPI-ACS Data 

This note describes how to quickly access INTEGRAL SPI-ACS data (including calibrated timing) expressed in simple formats (such as CSV, SSV). 
In addition access to satelite ephemeris is provided.

## Legacy IPN interface

This interface was built for sharing SPI-ACS to the IPN and is operational [since 2011](https://doi.org/10.13097/archive-ouverte/unige:23133)
The interface will remain operational until all the necessary features are reliably available by other similar means: as confirmed by the IPN. However, this legacy interface will not be further developed (for example, realtime data will not be added here).

The interface can be access at:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl

For a quick preview of a plot:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs-plot.pl?requeststring=2020-04-28T14:34:33+10&submit=Submit

For an "API" access:

```sh
$ curl 'https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2020-03-30T09%3A51%3A05+10&generate=ipnlc&submit=Submit'
```

## Latency and availablility

All SPI-ACS data is public. The service provides the data as soon as it is decoded from the telemetry into the near-realtime [Science Window](https://heasarc.gsfc.nasa.gov/W3Browse/integral/intscwpub.html) data.

## In case of issues

Consult [status page](https://status.reproducible.online/) for any announcements.
Contact me (the contact information should be shown on the page when the request can not be completed).

## List of known triggers

https://www.isdc.unige.ch/integral/ibas/cgi-bin/ibas_acs_web.cgi

## Accessing SPI-ACS data from MMODA

INTEGRAL data, including SPI-ACS, as well as various added-value products from other instruments can be accessed through [MMODA platform](https://doi.org/10.1051/0004-6361/202037850):

For quick visualization, interactive frontend is useful:

https://www.astro.unige.ch/mmoda/

It is also possible to use python API:

```python
from oda_api.api import DispatcherAPI

d = DispatcherAPI(url='https://www.astro.unige.ch/mmoda/dispatch-data').get_product(
    T1="2017-03-06T13:26:48.000",
    T2="2017-03-06T15:32:27.000",
    T_format="isot",
    instrument="spi_acs",
    product="spi_acs_lc",
    product_type="Real",
    time_bin=2.0,
    time_bin_format="sec")
    
print(d.spi_acs_lc_0_query.data_unit[1].data)
```

## Additional data

INTEGRAL ephemeries can be accessed here:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2021-10-14T19%3A00%3A00&generate=ephs&submit=Submit

Note that it is also possible to request predicted ephemeries, in the future. In this case, predicted orbit is used. Note that predicted attitude is also used for very recent data (hours old).

# Extra Metadata

|  | |
| --- | :-- |
| AKA | https://gitlab.unige.ch/Volodymyr.Savchenko/integral-spi-acs-data | 
| AKA | https://gitlab.astro.unige.ch/savchenk/integral-spi-acs-data | 

