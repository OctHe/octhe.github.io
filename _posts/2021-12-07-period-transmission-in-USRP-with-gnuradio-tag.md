---
layout: post
title: "Period Transmission in USRP with GNURadio Tag"
usemathjax: true
---

# Introduction

Period transmission in GNURadio/USRP platform is the basic function to support time-related systems, such as 
- Time delay measurement between multiple USRPs
- Time synchronization in designing distributed MIMO systems
- TX/RX switch at the same antenna
- Feedback design

The period transmission is based on [burst transmission](https://wiki.gnuradio.org/index.php/USRP_Sink) with "tx_time" tag.
All the functions have been tested on USRP N310 with GNURadio 3.7.13 and UHD 3.15.

# GNURadio Tag

[GNURadio tag](https://wiki.gnuradio.org/index.php/Stream_Tags) is an isosynchronous data that runs parallel to the main data stream. 

uhd_sink block supports two tags "tx_sob" and "tx_eob" for burst transmission.
"tx_sob" tag means "start of burst" that should be added into the first sample of a frame burst.
"tx_eob" means "end of burst" that should be added into the last sample.
If UHD receives "tx_eob", the TX mode of USRP will be done, until it receives a "tx_sob" tag.
The "tx_sob" and "tx_eob" value should simply be PMT_T.
As an example, user can use `add_item_tag` provided by GNURadio to add the "tx_sob" tag in C++.

    add_item_tag(0, // Port number
                 nitems_written(0) + i, // Offset
                 pmt::mp("tx_sob"), // Key
                 pmt::PMT_T // Value
            );

In the above code, the "tx_sob" tag will be added at port 0 with `nitems_written(0) + i` offset.

```
Note: add tag can also in python, but signal processing in C++ will 
have better performance.
```

Please refer to [this page](https://wiki.gnuradio.org/index.php/Stream_Tags) for a detail description about GNURadio tag.

The third important tag is "tx_time" which is a PMT encoded tuple with two components. 
The first item in the tuple is the PMT encoded uint64 epoch time in seconds. 
The second item in the tuple is the PMT encoded double fractional epoch time. 
The following example can be used to transfer the time from double to a PMT tuple in C++.

    double d_next_time = 1.0;
    uint64_t t_int = (uint64_t)d_next_time;
    double t_double = d_next_time - t_int;
    pmt::pmt_t tstamp_tuple = pmt::make_tuple(pmt::from_uint64(t_int), pmt::from_double(t_double));            

The "tx_time" tag will be added at port 0 with `nitems_written(0) + i` offset.

    add_item_tag(0, // Port number
                 nitems_written(0) + i, // Offset
                 pmt::mp("tx_time"), // Key
                 tstamp_tuple // Value
            );

```
Note: If the user directly add the "tx_time" tag with the offset is zero, 
the USRP will have a bug with unknown reason that the first two burst
frames are not sent with the predefined time by "tx_time".
```

With the help of the above tags, the user can control the transmit time of a burst frame with a fix length, so it can be used to implement period transmission.

# USRP Time Control

The last things is to set the time of USRP with gr-uhd, if the time has error, you may get a `L` error in the transmitter. 
`L` means a late packet in the TX chain. 
More details please refer to [System Configuration for USRP X3x0 Series](https://files.ettus.com/manual/page_usrp_x3x0_config.html).

The following code snapshot is used for time setting for both the USRP source and sink in python API.

    # Set time to 0 for each USRP
    self.uhd_usrp_sink_0.set_time_next_pps(uhd.time_spec(0.0))
    self.uhd_usrp_source_0.set_time_next_pps(uhd.time_spec(0.0))

    time.sleep(1) # Wait for the PPS signal

    # USRP sink sends at time 2.0
    # USRP source receives at time 1.0
    # This time can be set to any other times
    self.uhd_usrp_sink_0.set_start_time(uhd.time_spec(2.0))
    self.uhd_usrp_source_0.set_start_time(uhd.time_spec(1.0))

