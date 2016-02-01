# whatpulsed

## Usage

### First run

1. Create "whatpulsed.conf":
    * Write it based on the file description below  
      or
    * Copy and modify the example
2. Run with `python3 whatpulsed.py`
3. Wait for "whatpulsed.json" to be created
4. *(optional)* Remove plaintext login details from "whatpulsed.conf" by deleting `login` section

### Future runs

1. Run with `python3 whatpulsed.py`
2. Manually pulse by sending the process a `SIGUSR1`
3. Gracefully stop whatpulsed by sending the process a `SIGTERM`

## Files

### whatpulsed.conf

This is the configuration file for whatpulsed. It is in **INI format** and has the following sections:

* `login` - WhatPulse login details (optional, only required when no state file "whatpulsed.json" exists)
    - `email` - WhatPulse login email
    - `password` - WhatPulse login password
    - `computer` - computer name to log into
* `state` - state options (optional, but recommended)
    - `interval` *(time)* - state saving interval
* `interfaces` - network interfaces
    - list of interfaces to measure (no INI values), e.g. `eth0`
* `inputs` - evdev inputs
    - list of input devices to count (no INI values), e.g. `/dev/input/event2`
* `pulse` - automatic pulsing conditions (any satisfied condition pulses)
    - `keys` *(general)* - key count to pulse on
    - `clicks` *(general)* - click count to pulse on
    - `download` *(size)* - download amount to pulse on
    - `upload` *(size)* - upload amount to pulse on
    - `uptime` *(time)* - uptime to pulse on

#### converter

Some values can be in converted format, allowing more human-readable values:
* *(general)* - generic magnitude units

    | unit | meaning | value |
    | ---- | ------- | ----- |
    | k    | kilo    | 1000  |
    | m    | mega    | 1000k |
    | g    | giga    | 1000m |
    | t    | tera    | 1000g |
    e.g. `1m500k` means "1.5 million"
* *(size)* - base 2 magnitude units

    | unit | meaning | value |
    | ---- | ------- | ----- |
    | k    | kibi    | 1024  |
    | m    | mebi    | 1024k |
    | g    | gibi    | 1024m |
    | t    | tebi    | 1024g |
    e.g. `1g500m` means "1.5 gigabytes"
* *(time)* - time magnitude units

    | unit | meaning | value |
    | ---- | ------- | ----- |
    | s    | second  | 1     |
    | m    | minute  | 60s   |
    | h    | hour    | 60m   |
    | d    | day     | 24h   |
    | w    | week    | 7d    |
    | y    | year    | 52w   |
    e.g. `3d7h10m` means "3 days, 7 hours and 10 minutes"

### whatpulsed.json

This is the state file for whatpulsed and is not meant to be directly manipulated. It is in **JSON format** and is structured as follows:
* `login` - logged in state
    - `userid`
    - `computerid`
    - `hash`
    - `token`
* `stats` - unpulsed stats
    - `keys`
    - `clicks`
    - `download`
    - `upload`
    - `upime`

# upload_computerinfo

## Usage

1. Provide WhatPulse login details:
    1. Have "whatpulsed.json" as the result of running whatpulsed  
       or
    2. Create "whatpulsed.conf" as described above (only `login` section is required)
2. Create "computerinfo.json":
    * Write it based on the file description below  
      or
    * Copy and modify the example
3. Run with `python3 upload_computerinfo.py`

## Files

### computerinfo.json

This is the computer info file for upload_computerinfo. It is in **JSON format** and is structured similarly to the JSON object used for upload_computerinfo request in the API:
```json
{
    "VideoInfo": "NVIDIA GeForce GT 440",
    "TrackpadInfo": {},
    "NetworkInfo":
    {
        "isNetworkMonitorSupported":
        {
            "isNetworkMonitorSupported": true
        }
    },
    "CPUInfo": "Intel Core i5-2310",
    "ComputerModel": "",
    "ComputerOS": "Arch Linux",
    "ComputerPlatform": "x86_64",
    "KeyboardInfo": {},
    "MemoryInfo": 5955,
    "MonitorInfo":
    {
        "0":
        {
            "id": 0,
            "width": 1920,
            "height": 1080,
        },
        "1":
        {
            "id": 1,
            "width": 1280,
            "height": 1024,
        }
    },
    "MouseInfo": {}
}
```
where the values are:
* `VideoInfo` - GPU name
* `TrackpadInfo` - JSON object in unknown format, usually `{}`
* `NetworkInfo` - JSON object in the following format:

    ```json
    {
        "isNetworkMonitorSupported":
        {
            "isNetworkMonitorSupported": true
        },

        "Intel 82540EM Gigabit":
        {
            "is_wifi": false,
            "description": "Intel 82540EM Gigabit"
        },
        ...
    }
    ```
    where network cards as keys have values:
    - `is_wifi` - network card Wi-Fi ability
    - `description` - network card name, same as key
* `CPUInfo` - CPU name
* `ComputerModel` - computer model name, usually empty
* `ComputerOS` - operating system name
* `ComputerPlatform` - computer platform identifier, usually `i686` or `x86_64`
* `KeyboardInfo` - JSON object in unknown format, usually `{}`
* `MemoryInfo` - RAM amount in megabytes
* `MonitorInfo` - JSON object in the following format:

    ```json
    {
        "0":
        {
            "id": 0,
            "width": 1920,
            "height": 1080,
        },
        ...
    }
    ```
    where monitor IDs as keys have values:
    - `id` - monitor ID, same as key
    - `width` - monitor width
    - `height` - monitor height
* `MouseInfo` - JSON object in unknown format, usually `{}`
