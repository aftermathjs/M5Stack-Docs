Analog to Digital Converter
===========================

Overview
--------

ESP32  integrates two 12-bit SAR (`Successive Approximation Register <https://en.wikipedia.org/wiki/Successive_approximation_ADC>`_) ADCs (Analog to Digital Converters) and supports measurements on 18 channels (analog enabled pins). Some of these pins can be used to build a programmable gain amplifier which is used for the measurement of small analog signals.

The ADC driver API supports ADC1 (8 channels, attached to GPIOs 32 - 39), and ADC2 (10 channels, attached to GPIOs 0, 2, 4, 12 - 15 and 25 - 27).
However, there're some restrictions for the application to use ADC2:

1. The application can use ADC2 only when Wi-Fi driver is not started, since the ADC is also used by the Wi-Fi driver, which has higher priority.
2. Some of the ADC2 pins are used as strapping pins (GPIO 0, 2, 15), so they cannot be used freely. For examples, for official Develop Kits:


Configuration and Reading ADC
-----------------------------

The ADC should be configured before reading is taken.

 - For ADC1, configure desired precision and a  ttenuation by calling functions :cpp:func:`adc1_config_width` and :cpp:func:`adc1_config_channel_atten`. 
 - For ADC2, configure the attenuation by :cpp:func:`adc2_config_channel_atten`. The reading width of ADC2 is configured every time you take the reading.
 
Attenuation configuration is done per channel, see :cpp:type:`adc1_channel_t` and :cpp:type:`adc2_channel_t`, set as a parameter of above functions.

Then it is possible to read ADC conversion result with :cpp:func:`adc1_get_raw` and :cpp:func:`adc2_get_raw`. Reading width of ADC2 should be set as a parameter of :cpp:func:`adc2_get_raw` instead of in the configuration functions.

.. note:: Since the ADC2 is shared with the WIFI module, which has higher priority, reading operation of :cpp:func:`adc2_get_raw` will fail between :cpp:func:`esp_wifi_start()` and :cpp:func:`esp_wifi_stop()`. Use the return code to see whether the reading is successful.

It is also possible to read the internal hall effect sensor via ADC1 by calling dedicated function :cpp:func:`hall_sensor_read`. Note that even the hall sensor is internal to ESP32, reading from it uses channels 0 and 3 of ADC1 (GPIO 36 and 39). Do not connect anything else to these pins and do not change their configuration. Otherwise it may affect the measurement of low value signal from the sesnor.


There is another specific function :cpp:func:`adc2_vref_to_gpio` used to route internal reference voltage to a GPIO pin. It comes handy to calibrate ADC reading and this is discussed in section :ref:`adc-api-adc-calibration`.


