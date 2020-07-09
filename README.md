# timeguard-supplymaster

This repository contains information I have discovered about the API used by Timeguard's Supplymaster application. This is typically used to control [Timeguard's FSTWIFI Wi-Fi Controlled Fused Spur](https://www.timeguard.com/products/time/immersion-and-general-purpose-timeswitches/wi-fi-controlled-fused-spur)

## Legal Disclaimer

This information is un-official and is not endorsed or associated with Timeguard Limited in any way shape or form.

This information has been gathered legally using the Supplymaster Android application and [Charles Proxy](https://www.charlesproxy.com).

This information has been captured to aid my own personal efforts to automate scheduling of my FSTWIFI device for [Octopus Agile](https://octopus.energy/agile/)

The information is provided “as is”, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. in no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the informations or the use or mis-used or other dealings in the information.

## List of libraries that implement this API

| Language | URL | Status |
|----------|-----|--------|
| Python   | [https://github.com/rjpearce/timeguard-supplymaster-python](https://github.com/rjpearce/timeguard-supplymaster-python) | Under development |

## Authentication

This is required in order to obtain a token that you can then use to make further requests

```bash
TG_USERNAME=''
TG_PASSWORD=''

curl --user-agent "okhttp/3.3.1" -d "username=${TG_USERNAME}&password=${TG_PASSWORD}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/users/login" | python3 -m json.tool

{
    "status": true,
    "message": {
        "user": {
            "id": "12345",
            "username": "redacted",
            "email": "redacted",
            "add_time": "2019-07-03 16:22:14",
            "token": "redacted",
            "token_add_time": "2020-07-04 21:58:18",
            "is_sub_account": 0
        },
        "wifi_box": [
            {
                "device_id": "0123456789",
                "name": "My Device",
                "online": "1"
            }
        ]
    },
    "error_code": 0
}
```

Grab the user_id, token as they are needed to make other requests.

You can also grab the device_id to save having to request a list of all devices.

```bash
export TG_TOKEN='redacted'
export TG_USER_ID='12345'
export TG_DEVICE_ID='0123456789'
```

## Devices

### List Devices

```bash

curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/users/wifi_boxes/user_id/${TG_USER_ID}/is_sub_user/0/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": [
        {
            "device_id": "0123456789",
            "name": "My Device",
            "online": "1",
            "codeversion": "4190117051001",
            "relay": "1",
            "work_mode": "0",
            "advance": "0",
            "main_relay": {
                "loaded": "1",
                "work_status": "0"
            }
        }
    ],
    "error_code": 0
}

```

## Device

### Get Information

The Map shown in the output represents the day of the week.

| Map | Day       |
|-----|-----------|
| 0   | Sunday    |
| 1   | Monday    |
| 2   | Tuesday   |
| 3   | Wednesday |
| 4   | Thursday  |
| 5   | Friday    |
| 6   | Saturday  |

```bash
curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/data/user_id/${TG_USER_ID}/wifi_box_id/${TG_DEVICE_ID}/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": {
        "relay": "1",
        "time_zone": "0",
        "pro_index": "0",
        "work_mode": "0",
        "advance": "0",
        "boost": {
            "hour": "0",
            "start_time": "0"
        },
        "holiday": {
            "enable": "0",
            "end": "1970-01-01 00:00:00",
            "start": "1970-01-01 00:00:00"
        },
        "main_relay": {
            "loaded": "1",
            "work_status": "0"
        },
        "program": [
            {
                "end": {
                    "enable": "1",
                    "time": "08:30"
                },
                "map": {
                    "0": "1",
                    "1": "1",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "08:00"
                }
            },
            {
                "end": {
                    "enable": "1",
                    "time": "16:00"
                },
                "map": {
                    "0": "1",
                    "1": "1",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "15:00"
                }
            },
            {
                "end": {
                    "enable": "1",
                    "time": "14:00"
                },
                "map": {
                    "0": "1",
                    "1": "1",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "13:30"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            }
        ]
    },
    "error_code": 0
}
```

### Set Device Name

* Must start with a letter
* Must be betweeen 5 to 19 characters
* Allowed: Letters, Number and Spaces

```bash
export TG_NAME='My Device'
curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}&name=${TG_NAME}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/name"
```

## Programs

* A program is effectively just a schedule
* A device can have upto 5 programs (schedules)
* Each program can have 6 on/off times
* A device can only have 1 program active
* Deleting a program does not re-index existing programs

### List Programs

```bash
curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program_list/user_id/${TG_USER_ID}/wifi_box_id/${TG_DEVICE_ID}/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": {
        "choosex": "0",
        "namelist": [
            {
                "id": "0",
                "name": "Schedule 0"
            },
            {
                "id": "1",
                "name": ""
            },
            {
                "id": "2",
                "name": ""
            },
            {
                "id": "3",
                "name": ""
            },
            {
                "id": "4",
                "name": ""
            },
            {
                "id": "5",
                "name": ""
            }
        ]
    },
    "error_code": 0
}

```

### Get Program

```bash
export TG_PROGRAM_ID='0'
curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program/user_id/${TG_USER_ID}/wifi_box_id/${TG_DEVICE_ID}/index/${TG_PROGRAM_ID}/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": {
        "id": "0",
        "name": "Default",
        "program": [
            {
                "end": {
                    "enable": "1",
                    "time": "05:00"
                },
                "map": {
                    "0": "1",
                    "1": "0",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "04:30"
                }
            },
            {
                "end": {
                    "enable": "1",
                    "time": "04:00"
                },
                "map": {
                    "0": "1",
                    "1": "1",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "03:30"
                }
            },
            {
                "end": {
                    "enable": "1",
                    "time": "07:30"
                },
                "map": {
                    "0": "1",
                    "1": "1",
                    "2": "1",
                    "3": "1",
                    "4": "1",
                    "5": "1",
                    "6": "1"
                },
                "start": {
                    "enable": "1",
                    "time": "07:00"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            },
            {
                "end": {
                    "enable": "0",
                    "time": "00:00"
                },
                "map": {
                    "0": "0",
                    "1": "0",
                    "2": "0",
                    "3": "0",
                    "4": "0",
                    "5": "0",
                    "6": "0"
                },
                "start": {
                    "enable": "0",
                    "time": "00:00"
                }
            }
        ]
    },
    "error_code": 0
}
```

### Update Program

The Map shown in the output represents the day of the week.

| Map | Day       |
|-----|-----------|
| 0   | Sunday    |
| 1   | Monday    |
| 2   | Tuesday   |
| 3   | Wednesday |
| 4   | Thursday  |
| 5   | Friday    |
| 6   | Saturday  |

```bash
read -r -d '' TG_PROGRAM <<'EOF'
{
    "id": "2",
    "name": "My Period",
    "program": [
        {
            "end": {
                "enable": "1",
                "time": "08:30"
            },
            "map": {
                "0": "1",
                "1": "1",
                "2": "1",
                "3": "1",
                "4": "1",
                "5": "1",
                "6": "1"
            },
            "start": {
                "enable": "1",
                "time": "06:30"
            }
        },
        {
            "end": {
                "enable": "0",
                "time": "00:00"
            },
            "map": {
                "0": "0",
                "1": "0",
                "2": "0",
                "3": "0",
                "4": "0",
                "5": "0",
                "6": "0"
            },
            "start": {
                "enable": "0",
                "time": "00:00"
            }
        },
        {
            "end": {
                "enable": "0",
                "time": "00:00"
            },
            "map": {
                "0": "0",
                "1": "0",
                "2": "0",
                "3": "0",
                "4": "0",
                "5": "0",
                "6": "0"
            },
            "start": {
                "enable": "0",
                "time": "00:00"
            }
        },
        {
            "end": {
                "enable": "0",
                "time": "00:00"
            },
            "map": {
                "0": "0",
                "1": "0",
                "2": "0",
                "3": "0",
                "4": "0",
                "5": "0",
                "6": "0"
            },
            "start": {
                "enable": "0",
                "time": "00:00"
            }
        },
        {
            "end": {
                "enable": "0",
                "time": "00:00"
            },
            "map": {
                "0": "0",
                "1": "0",
                "2": "0",
                "3": "0",
                "4": "0",
                "5": "0",
                "6": "0"
            },
            "start": {
                "enable": "0",
                "time": "00:00"
            }
        },
        {
            "end": {
                "enable": "0",
                "time": "00:00"
            },
            "map": {
                "0": "0",
                "1": "0",
                "2": "0",
                "3": "0",
                "4": "0",
                "5": "0",
                "6": "0"
            },
            "start": {
                "enable": "0",
                "time": "00:00"
            }
        }
    ]
}
EOF

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&program=${TG_PROGRAM}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}

```

### Enable Program

Program index starts at 0 and is not always sequential if programs have been deleted
Validate program index exists or it will display with Unknown Name and as soon as you make it inactive it will disapeer

```bash
export TG_PROGRAM_ID='1'

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&index=${TG_PROGRAM_ID}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program_enable"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}
```

### Set Name for Program

```bash
export TG_PARA='{"name":"My Program","id":"2"}'

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&user_id=${TG_USER_ID}&para=${TG_PARA}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program_name"
```

## Mode

### Get Mode

| Work Mode | Name                                        |
|-----------|---------------------------------------------|
| 0         | Auto timed (use the active Period/Schedule) |
| 1         | Always Off                                  |
| 2         | Always On                                   |
| 3         | Holiday                                     |

```bash
curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/mode_and_holiday/user_id/${TG_USER_ID}/wifi_box_id/${TG_DEVICE_ID}/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": {
        "work_mode": "0",
        "holiday": {
            "enable": "0",
            "end": "2020-08-13 00:01:00",
            "start": "2020-07-09 21:21:00"
        }
    },
    "error_code": 0
}
```

### Set Mode

| Work Mode | Name                                        |
|-----------|---------------------------------------------|
| 0         | Auto timed (use the active Period/Schedule) |
| 1         | Always Off                                  |
| 2         | Always On                                   |
| 3         | Holiday                                     |

Note: Holiday (Will only stay set if holiday return date is enable=1)

```bash
export TG_MODE=0
curl --user-agent "okhttp/3.3.1" -d "work_mode=${TG_MODE}&token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/work_mode"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}
```

## Boost

Turn on the device for 1 hour (non-configurable)

```bash
export TG_BOOST=1
curl --user-agent "okhttp/3.3.1" -d "boost=${TG_BOOST}&token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/boost"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}
```

## Advance

Turn on the device until the next scheduled off time

```bash
export TG_ADVANCE=1
curl --user-agent "okhttp/3.3.1" -d "advance=${TG_ADVANCE}&token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/advance"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}
```

## Holiday

### Get Holiday Return Date

```bash
curl --user-agent "okhttp/3.3.1" -X GET "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/holiday/user_id/${TG_USER_ID}/wifi_box_id/${TG_DEVICE_ID}/token/${TG_TOKEN}" | python3 -m json.tool

{
    "status": true,
    "message": {
        "enable": "1",
        "end": "2020-08-13 00:01:00",
        "start": "2020-07-09 21:10:00"
    },
    "error_code": 0
}
```

### Set Holiday Return Date

If you set enable=1 it will also change the device mode to 3 (Holiday)

```bash
export TG_HOLIDAY='{"end":"2020-08-13 00:01:00","enable":"1","start":"2020-07-09 21:10:38"}'
curl --user-agent "okhttp/3.3.1" -d "holiday=${TG_HOLIDAY}&token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/holiday"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}
```

## Historical Data

### Get Historical Data

```bash
export TG_START_TIME='2020-07-08%2020:13:53'
export TG_END_TIME='2020-07-09%2020:13:53'
curl --user-agent "okhttp/3.3.1" -d "start_time=${TG_START_TIME}&end_time=${TG_END_TIME}&token=${TG_TOKEN}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/historical_data" | python3 -m json.tool

{
    "status": true,
    "message": [
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 02:30:00",
            "server_time": "2020-07-09 03:30:00"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 02:30:04",
            "server_time": "2020-07-09 03:30:04"
        },
        {
            "relay_status": "1",
            "load_status": "0",
            "change_type": "3",
            "device_time": "2020-07-09 03:00:00",
            "server_time": "2020-07-09 04:00:01"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 03:30:00",
            "server_time": "2020-07-09 04:30:00"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 03:30:04",
            "server_time": "2020-07-09 04:30:04"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "2",
            "device_time": "2020-07-09 03:55:00",
            "server_time": "2020-07-09 04:55:00"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 03:57:57",
            "server_time": "2020-07-09 04:57:57"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "2",
            "device_time": "2020-07-09 03:58:44",
            "server_time": "2020-07-09 04:58:44"
        },
        {
            "relay_status": "1",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 04:00:00",
            "server_time": "2020-07-09 05:00:01"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 05:59:59",
            "server_time": "2020-07-09 06:59:59"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 06:00:04",
            "server_time": "2020-07-09 07:00:05"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "2",
            "device_time": "2020-07-09 06:07:32",
            "server_time": "2020-07-09 07:07:32"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 06:16:50",
            "server_time": "2020-07-09 07:16:50"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "2",
            "device_time": "2020-07-09 06:17:44",
            "server_time": "2020-07-09 07:17:45"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 06:25:20",
            "server_time": "2020-07-09 07:25:20"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "2",
            "device_time": "2020-07-09 06:26:18",
            "server_time": "2020-07-09 07:26:18"
        },
        {
            "relay_status": "1",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 06:30:00",
            "server_time": "2020-07-09 07:30:01"
        },
        {
            "relay_status": "2",
            "load_status": "0",
            "change_type": "1",
            "device_time": "2020-07-09 20:00:52",
            "server_time": "2020-07-09 21:00:52"
        },
        {
            "relay_status": "2",
            "load_status": "1",
            "change_type": "2",
            "device_time": "2020-07-09 20:00:56",
            "server_time": "2020-07-09 21:00:56"
        },
        {
            "relay_status": "1",
            "load_status": "0",
            "change_type": "3",
            "device_time": "2020-07-09 20:05:23",
            "server_time": "2020-07-09 21:05:23"
        }
    ],
    "error_code": 0
}
```
