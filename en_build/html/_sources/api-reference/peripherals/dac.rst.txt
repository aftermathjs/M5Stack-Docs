Digital To Analog Converter
===========================

Overview
--------

ESP32 has two 8-bit DAC (digital to analog converter) channels, connected to GPIO25 (Channel 1) and GPIO26 (Channel 2).

The DAC driver allows these channels to be set to arbitrary voltages.

The DAC channels can also be driven with DMA-style written sample data, via the :doc:`I2S driver <i2s>` when using the "built-in DAC mode".

For other analog output options, see the :doc:`Sigma-delta Modulation module <sigmadelta>` and the :doc:`LED Control module <ledc>`. Both these modules produce high frequency PWM output, which can be hardware low-pass filtered in order to generate a lower frequency analog output.

