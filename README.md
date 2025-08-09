# Pocket Synth

[Online Board & Schematic View](https://kicanvas.org/?github=https%3A%2F%2Fgithub.com%2Fnorthernpaws%2Fpocketsynth%2Fblob%2Fmain%2Fhardware%2Fpocket_synth.kicad_pro)

This repository contains my project for an STM32H7-based sequencer, synthesizer, and sampler, all in a small phone-sized pocketable form factor. 

  * 480 MHz Arm® Cortex®-M7 32-bit Processor (`STM32H743ZIT6`)
    * Floating-point unit
    * DSP instructions
    * High-speed memory controller
    * Quad SPI interface for flash
    * High-speed USB host and device mode
    * Several SAI audio blocks
  * 2.4" 240x320 TFT IPS Display (`ER-TFT024IPS-3`)
    * Capacitive touch
  * 24 bit Audio Codec (`WM8904CGEFL/RV`)
    * DAC SNR 96dB
    * ADC SNR 91dB
  * Optional Bluetooth Classic and BLE Audio Source (`FSC-BT6038`)
    * Wirelessly connect Bluethooth headphones and speakers
    * Supports various high-fedality audio protocols
    * Note: Audio transmitter does not support other BLE protocols
  * Optional Wifi and BLE capabilities (`ESP32-C3-MINI-1`)
    * Wifi sample management and transfer
    * BLE MIDI
  * Interal speaker
  * Internal microphone
  * SDRAM for sample pool and audio processing
    * Part allocated to in-memory sample pool for project
    * Part allocated to audio effects processing
    * Variable sizes in similar footprint available, up to 512Mbit:
        * 16bit * 48000Hz = 768,000b/s = 0.768Mb/s
        * 128Mb / 0.768Mb/s = 166.66 seconds of mono audio (`IS42S16800J-7TL`)
        * 256Mb / 0.768Mb/s = 333.33 seconds of mono audio (`IS42S16160J-7TLI`)
        * 512Mb / 0.768Mb/s = 666.66 seconds of mono audio (`IS42S16320F-7TLI`)
  * Micro SD Card
    * Saving and loading projects
      * Transfer and back up projects to a computer
    * Saving and loading samples
      * Load samples from a computer
      * Save samples recorded on the device
  * 3.3v NOR Flash (`MT25QUxxxxxx1xW7` 256M/128M/64M)
    * Stores device settings
    * Small storage space for projects and samples without SD card
    * Provides storage for cross-SD card samples
  * USB-C
    * USB-PD fast charging
    * Possible future audio interface mode
  * MIDI in/out via TRS minijack
  * 16 Multi-Purpose primary keys
    * Hot-swappable Kalih Choc keyswitches (V1 and V2 compatible)
    * RGB LEDs for state indicators
  * Accelerometer and Gyroscope (`LSM6DS3`)
    * Can be used as sequencer parameters
    * Can be recorded for automation inputs
    * Used for auto sleep/wake
  * USB-PD Battery Charge Controller (`MAX77789` or `MAX77757`)
    * 3A fast-charge current
    * 13.4V input voltage
    * Detection for legacy SDP, DCP, and CDP chargers
  * Two-Button Smart Reset (`STM6510`)
    * Hold two input buttons to power off the device if it freezes and power gets latched on

## Audio

The primary audio engine chain runs at 16bit 44.1Khz, but supports stereo audio effects. 16 bit audio was selected as it had suitable depth without being too large for a small embedded device to reasonably process. 44.1Khz also has a good frequency range, and has been the standard for CDs and many music streaming platforms for a long time.

### Sampling

The sampling engine runs 16bit 44.1Khz and saves and loads samples in single-channel mono.

Samples recoded from the internal or external mic are sampled at 24 bits into the device's SDRAM to provide a decent noise floor, and then filtered and downsampled to 16 bits for saving after they've been edited by the user.

The 16bit @ 44.1Khz sampling parameters provdes a good balance between audio quality, and reasonable utilization of the SDRAM without needing extremely large and expensive SDRAM chips:

```
16bit * 48000Hz = 768,000b/s = 0.768Mb/s
128Mb / 0.768Mb/s = 166.66 seconds of mono audio
256Mb / 0.768Mb/s = 333.33 seconds of mono audio
512Mb / 0.768Mb/s = 666.66 seconds of mono audio
```

> Note that some of the space calculated above (actual amount will be configurable) is allocated to the audio effects processing chain, i.e. for reverb and delay. 


## Attribution

### 3D Models

Majority of 3D models for PCB components are from their respective manufactures, with the exception of:

  - https://grabcad.com/library/kailh-low-profile-mechanical-keyboard-switch-1
  - https://grabcad.com/library/kailh-low-profile-keycaps-1
  - https://cad.grabcad.com/library/sk6812mini-e-led-1

