# timeguard-supplymaster

This repository contains information I have discovered about the API used by Timeguard's Supplymaster application. This is typically used to control [Timeguard's FSTWIFI Wi-Fi Controlled Fused Spur](https://www.timeguard.com/products/time/immersion-and-general-purpose-timeswitches/wi-fi-controlled-fused-spur)

## Legal Disclaimer

This information is un-official and is not endorsed or associated with Timeguard Limited in any way shape or form.

This information has been gathered legally using the Supplymaster Android application and [Charles Proxy](https://www.charlesproxy.com).

This information has been captured to aid my own personal efforts to automate scheduling of my FSTWIFI device for [Octopus Agile](https://octopus.energy/agile/)

The information is provided “as is”, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. in no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the informations or the use or mis-used or other dealings in the information.

## Login

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

You will need to retain the user_id and token to make further requests

## Devices

### List Devices

```bash
export TG_TOKEN=''
export TG_USER_ID=''

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

### Device - Get Info

```bash
export TG_TOKEN=''
export TG_USER_ID=''
export TG_DEVICE_ID=''

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

### Device - List Programs

* Each device can have upto 5 programs (schedules)
* Only 1 program can be active at a time

```bash
export TG_TOKEN=''
export TG_USER_ID=''
export TG_DEVICE_ID=''

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

### Device - Update Program x

```bash
export TG_TOKEN=''
export TG_USER_ID=''
export TG_DEVICE_ID=''
export TG_PROGRAM='x'

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&index=${TG_PROGRAM}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program"

{
  "status": true,
  "message": "Set successfully",
  "error_code": 0
}

```

### Device - Enable Program x

```bash
export TG_TOKEN=''
export TG_USER_ID=''
export TG_DEVICE_ID=''
export TG_PROGRAM='x'

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&index=${TG_PROGRAM}&user_id=${TG_USER_ID}&wifi_box_id=${TG_DEVICE_ID}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program_enable"
```

### Device - Set Name for Program x

```bash
export TG_TOKEN=''
export TG_USER_ID=''
export TG_DEVICE_ID=''
export TG_PARA='{"name":"My Program","id":"x"}'

curl --user-agent "okhttp/3.3.1" -d "token=${TG_TOKEN}&user_id=${TG_USER_ID}&para=${TG_PARA}" -X PUT "https://www.cloudwarm.net/TimeGuard/api/Android/v_1/wifi_boxes/program_name"
```