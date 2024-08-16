[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.7472097.svg)](https://doi.org/10.5281/zenodo.7472097)

# Accesing INTEGRAL SPI-ACS Data 

This note describes how to quickly access INTEGRAL SPI-ACS data (including calibrated timing) expressed in simple formats (such as CSV, SSV). 
In addition access to satelite ephemeris is provided.


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

The data is accompanied with an additonal comment in the start and the end of the current satellite orbit, setting boundaries of continious data taking. 

### Realtime SPI-ACS data in MMODA

Since XXXXX, MMODA also serves realtime SPI-ACS data with latency of about 20 seconds.

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

## Absolute timing

### Time system scale

Time in SPI-ACS, as in the rest of the INTEGRAL data, is expressed in INTEGRAL Julian Day (IJD), as described in [here](https://www.isdc.unige.ch/integral/support/faq.cgi?DATA-007).
**Please beware** that this time is expresse in Terrestrial Time (TT) and not in UTC. **It is a source of confusion** that for the same format (e.g. ISOT, MJD), the time scale is different. This scale difference depends on the current number of seconds, and as of 2023 accounts for `32.184 + 37` seconds.

### INTEGRAL ephemeris

It is often useful to derive time of the signal to some other place than INTEGRAL satellite, e.g. solar system barycenter, or Earth center. Time of light propagation between INTEGRAL and desired point depends on the coordinates source of the emission and INTEGRAL position (ephemeris).

INTEGRAL ephemeris can be obtained for any given time here

`https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2023-08-12T18%3A58%3A00&generate=ephs&submit=Submit` (an alternative is `https://www.astro.unige.ch/mmoda/dispatch-data/gw/integralhk/api/v1.0/ephs/2023-08-12T18:58:00`).

The result is something like:

`208.951 52.770 116867.9`

Where first two values are RA, Dec of INTEGRAL, and the last value is distance to the satellite in geocentric coordinate system.

### INTEGRAL Spacecraft system service

There is service making the barycenter calculation accessible in here (please check for yourself, please report any issues if found):

For example, here is a request for information at the time used in the previous section, and using source coordinates identical to INTEGRAL GCS coordinates.

`https://www.astro.unige.ch/mmoda/dispatch-data/gw/scsystem/api/v1.0/sc/2023-08-12T18:58:00/208.951/52.770`

This results in `barytime: 0.3895597` which is easy to check.

### ii_light 

If you have light curve in the right format you can https://www.isdc.unige.ch/integral/download/osa/doc/11.1/osa_um_ibis/node65.html

### Timing accuracy

Frequently used reference for INTEGRAL timing accuracy is [Kuiper 2023](https://ui.adsabs.harvard.edu/abs/2003A%26A...411L..31K/abstract). More recent study is presented [here](https://iachec.org/wp-content/presentations/2018/Kuiper_SessionXI.pdf) presenting absolute timing of Crab pulsar for 15 years of the mission.

## In case of issues

Consult [status page](https://status.reproducible.online/) for any announcements.
Contact me (the contact information should be shown on the page when the request can not be completed).

## List of known triggers

https://www.isdc.unige.ch/integral/ibas/cgi-bin/ibas_acs_web.cgi


## Additional data

INTEGRAL ephemeries can be accessed here:

https://www.isdc.unige.ch/~savchenk/spiacs-online/spiacs.pl?requeststring=2021-10-14T19%3A00%3A00&generate=ephs&submit=Submit

Note that it is also possible to request predicted ephemeries, in the future. In this case, predicted orbit is used. Note that predicted attitude is also used for very recent data (hours old).

## SPI-ACS data timing and its accuracy

### Time systems

INTEGAL data typically uses INTEGRAL Julian Date (IJD) defined as JD-2 451 544.5 or MJD-51 544.0. It starts on January 1st, 2000, **but expressed in Terrestrial Time (TT) and not in UTC**. Since TT differs from UTC by 32.184 sec + 32 leap seconds at the start of year 2000, **the UTC origin of the IJD is actually 1999-12-31 T23:58:55.816**. For additional information see [ISDC FAQ](https://www.isdc.unige.ch/integral/support/faq.cgi?DATA-007) and [Systems of Time](http://tycho.usno.navy.mil/systime.html).

### Timing accuracy

INTEGRAL data is recorded with on-board clocks and stored in OBT (on-board time) format.
INTEGRAL on-board clock is converted to other time systems through **time correlation tables**. These tables are generally available only with some delay following the data.

Realtime data receipt uses an additional **realtime time correlation tables**. The data using these tables (for example, the data distributed in [INTEGRAL SPI-ACS notices](https://gcn.gsfc.nasa.gov/integral_spiacs.html)) have reduced timing accuracy.

Realtime data served in [MMODA]() uses timing accuracy closer to that provided by the nominal **time correlation tables**. At this time, it is however advisable to re-check the timing accuracy once the near real-time data is available.


## Alerts distibuted with INTEGRAL SPI-ACS data

* https://gcn.gsfc.nasa.gov/integral_spiacs.html
* https://gcn.gsfc.nasa.gov/ipn/gcn_ipn_raw.html
* GCN circulars are hand-written messages. Since SPI-ACS data does not on its own provide sufficient localization information, these messages are written for SPI-ACS observations only in the case of exceptional events when the mere fact of detection is important. 

## INTEGRAL data readiness

* **Realtime** - decoded immediately from the telemetry which is available with ~second latency. Used in IBAS. Actually available data also depends on each  kind of the telemetry packet, for example housekeeping packets with SPI-ACS data are produced every 8 seconds and the data is available with about 20 s delay.
* **Near Real Time** - decoded from telemetry and stored in ScW version 000. Since telemetry can be interrupted, these data might be incomplete. Also the reconstruction is approximate due to lack of offline auxiliary information.
* **Consolidated** - decoded from data recieved in separate offline channel, and are "as complete and they will ever be". These data are archived, and sometimes revised. Currently INTEGRAL archives in 3rd revision ("rev_3").

## Extra Metadata

|  | |
| --- | :-- |
| AKA | https://gitlab.unige.ch/Volodymyr.Savchenko/integral-spi-acs-data | 
| AKA | https://gitlab.astro.unige.ch/savchenk/integral-spi-acs-data | 


https://ui.adsabs.harvard.edu/abs/2003A%26A...411L..31K/abstract
