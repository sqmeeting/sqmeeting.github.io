# API
SQMeeting Server provides public REST API for 3rdparty integration.

## Table of Contents
- [API](#api)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Common Section](#common-section)
    - [HTTP Common Headers](#http-common-headers)
    - [HTTP URL Common Parameters](#http-url-common-parameters)
  - [Meeting Schedule](#meeting-schedule)
    - [Create Non-recurrence Meeting](#create-non-recurrence-meeting)
    - [Update Non-recurrence Meeting](#update-non-recurrence-meeting)
    - [Delete Scheduled Meeting](#delete-scheduled-meeting)
    - [Get Scheduled Meeting Detail](#get-scheduled-meeting-detail)
    - [Get Scheduled Meetings](#get-scheduled-meetings)
    - [Create a Recurrence Meeting](#create-a-recurrence-meeting)
    - [Update a Recurrence Meeting](#update-a-recurrence-meeting)
    - [Get Recurrence Meetings](#get-recurrence-meetings)
  - [Meeting Control](#meeting-control)
    - [Mute All](#mute-all)
    - [Unmute All](#unmute-all)
    - [Mute One or More](#mute-one-or-more)
    - [Unmute One or More](#unmute-one-or-more)
    - [Stop Meeting](#stop-meeting)
    - [Get Meeting List](#get-meeting-list)
    - [Get Meeting Detail](#get-meeting-detail)
    - [Start Text Overlay](#start-text-overlay)
    - [Stop Text Overlay](#stop-text-overlay)
    - [Set Lecturer](#set-lecturer)
    - [Unset Lecturer](#unset-lecturer)
    - [Disconnect One or More Participants](#disconnect-one-or-more-participants)
    - [Disconnect All Participants](#disconnect-all-participants)
    - [Cascading](#cascading)
    - [Update Active Meeting](#update-active-meeting)
    - [Update Participant Info in Meeting](#update-participant-info-in-meeting)
    - [Start Recording](#start-recording)
    - [Stop Recording](#stop-recording)
    - [Start Live Streaming](#start-live-streaming)
    - [Stop Live Streaming](#stop-live-streaming)
    - [Pin for Participant](#pin-for-participant)
    - [Unpin for Participant](#unpin-for-participant)
    - [Pin for Meeting](#pin-for-meeting)
    - [Unpin for Meeting](#unpin-for-meeting)
    - [Layout Change for Participant](#layout-change-for-participant)
    - [Request Unmute](#request-unmute)
    - [Allow Unmute](#allow-unmute)
    - [Lock Meeting](#lock-meeting)
    - [Unlock Meeting](#unlock-meeting)
  - [User](#user)
    - [Sign In](#sign-in)
    - [Sign Out](#sign-out)
    - [User Info](#user-info)
    - [Update Password](#update-password)
    - [Find Users by Page](#find-users-by-page)
    - [Create User](#create-user)
    - [Update User](#update-user)
  - [Meeting Room](#meeting-room)
    - [Query Meeting List](#query-meeting-list)
  - [System](#system)
    - [Get System Capacity](#get-system-capacity)
    - [Get System Version](#get-system-version)

## Introduction

Frtc Server provides a lot of public APIs for 3rd party (app, sdk or IT managment system) to integrate, customize and enhance Frtc Solution.


## Common Section

### HTTP Common Headers
| Header | Value | Description |
|-----------|-----------|-----------|
| Content-Type | application/json | 1 |
| User-Agent   | FrtcMeeting/3.4.2 platform platform_version | 2 |


<!-- | Content-Type | application/json | The media type of the resource |
| User-Agent   | FrtcMeeting/3.4.2 platform platform_version | e.g. FrtcMeeting/3.4.2 windows 10.0.19045 | -->

### HTTP URL Common Parameters

**Signed In Query** `client_id=$client_id&token=$user_token`

**Unsigned In Query** `client_id=$client_id`

**Example1:** `/api/v1/meeting_schedule?client_id=$client_id&token=$user_token`

**Example2:** `/api/v1/user/sign_in?client_id=$client_id`

## Meeting Schedule

### Create Non-recurrence Meeting

**POST** `/api/v1/meeting_schedule`

**Request:**
```json
//for instant meeting 
{ 
 "meeting_type":"instant", 
 "meeting_name":"XX's meeting" 
}

//for reservation meeting 
{ 
 "meeting_type": "instant/reservation", 
 "meeting_name": "Someone's meeting", 
 "meeting_description": "XXXXXX", 
 "meeting_number": "{meeting_number}", 
 "call_rate_type": "2048K", 
 "schedule_start_time": 1641525383726, 
 "schedule_end_time": 1641532583788, 
 "meeting_password": "111111", 
 "invited_users_detail": [ 
  { 
   "user_id": "xxxxxxx_user_uuid", 
   "username": "test1", 
   "real_name": "Test1" 
  } 
 ], 
 "owner_name": "{owner_name}", 
 "reservation_id": "{reservation_id}", 
 "recurrence_gid": "{recurrence_gid}", 
 "recurrence_type": "NONE", 
 "generate_live_url": true, 
 "live_passcode": "888888", 
 "live_meeting_url": "https://xxxx/live/84ef2357-b460-4957-a41a
2c48aa8d68c6", 
 "time_to_join": 30, 
 "meeting_url": "https://xxxxx/j/oP5idgfEmt0p" 
} 
```

**Response:**
```json
//http 200
{ 
 "meeting_type": "instant/reservation", 
 "meeting_name": "Someone's meeting", 
 "meeting_description": "XXXXXX", 
 "meeting_number": "{meeting_number}", 
 "call_rate_type": "2048K", 
 "schedule_start_time": 1641525383726, 
 "schedule_end_time": 1641532583788, 
 "meeting_password": "111111", 
 "invited_users_detail": [ 
  { 
   "user_id": "{user_id}", 
   "username": "test1", 
   "real_name": "Test1" 
  } 
 ], 
 "owner_name": "$owner_name", 
 "reservation_id": "{reservation_id}", 
 "recurrence_gid": "{recurrence_gid}", 
 "recurrence_type": "NONE", 
 "generate_live_url": true, 
 "live_passcode": "888888", 
 "live_meeting_url": "https://xxxxx/live/84ef2357-b460-4957-a41a
2c48aa8d68c6", 
 "time_to_join": 30, 
 "meeting_url": "https://xxxxx/j/oP5idgfEmt0p" 
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**
| Field      | Type   | Description                        |
|------------|--------|------------------------------------|
| `meeting_type`       | int    | the type of meeting |
| `meeting_name`     | string | Name of the meeting               |

**Response:**
| Field      | Type   | Description                        |
|------------|--------|------------------------------------|
| `meeting_number`       | int    | the number of the meeting |

### Update Non-recurrence Meeting

**POST** `/api/v1/meeting_schedule/{reservation_id}`

**Request:**
```json
//for instant meeting can not be updated 
//only for reservation meeting 
{ 
"meeting_type": "reservation", 
"meeting_name": "$meeting_name", 
"meeting_description": "XXXXXX", 
"call_rate_type": "2048K", 
"schedule_start_time": "{schedule_start_time}", 
"schedule_end_time": "{schedule_end_time}", 
"meeting_room_id": null, 
"meeting_password": "123456", 
"invited_users":["{user_id}"], 
"mute_upon_entry": "DISABLE/ENABLE", 
"guest_dial_in": true/false, 
"watermark": false/true, 
"watermark_type": "single " 
}
```

**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Delete Scheduled Meeting

**DELETE** `/api/v1/meeting_schedule/{reservation_id}`

**Request:**


**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Get Scheduled Meeting Detail 

**GET** `/api/v1/meeting_schedule/{reservation_id}`

**Request:**


**Response:**
```json
//http 200
{
    "reservation_id": "{reservation_id}",
    "meeting_type": "reservation",
    "meeting_name": "{meeting_name}",
    "call_rate_type": "4096K",
    "recurrence_gid": "{recurrence_gid}",
    "schedule_start_time": "{schedule_start_time}",
    "schedule_end_time": "{schedule_end_time}",
    "recurrence_type": "NONE/DAILY/WEEKLY/MONTHLY",
    "meeting_room_id": "xxxxxxxxxxxx",
    "meeting_password": "{meeting_password}",
    "invited_users_detail":
    [
        {
            "user_id": "{user_id}",
            "username": "test1",
            "real_name": "Test1"
        }
    ],
    "mute_upon_entry": "DISABLE/ENABLE",
    "guest_dial_in": true,
    "watermark": false,
    "watermark_type": "single",
    "owner_id": "{user_id}",
    "owner_name": "xxxxx",
    "meeting_url": ""
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**


### Get Scheduled Meetings 

**GET** `/api/v1/meeting_schedule`

**Query**  `page_num={number}&page_size={page_size}&filter={search_key}`

**Request:**


**Response:**
```json
//http 200
{
    "scheduled_meetings":
    [
        {
            "meeting_number": "12345678",
            "meeting_name": "scheduled meeting name",
            "meeting_type": "reservation",
            "meeting_password": "{password}",
            "reservation_id": "",
            "recurrence_gid": "",
            "schedule_start_time": "{schedule_start_time}",
            "schedule_end_time": "{schedule_end_time}",
            "recurrence_type": "NONE",
            "owner_id": "{owner_id}",
            "owner_name": "{owner_name}",
            "meeting_url": "https://xxxx/j/oP5idgfEmt0p"
        }
    ]
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**

### Create a Recurrence Meeting 

**GET** `/api/v1/meeting_schedule/recurrence`

**Request:**
```json
{
    "recurrence_type": "DAILY",
    "meeting_name": "Someone's meeting",
    "meeting_description": "XXXXXX",
    "schedule_start_time": 1641525383726,
    "schedule_end_time": 1641532583788,
    "meeting_room_id": null,
    "call_rate_type": "2048K",
    "meeting_password": "111111",
    "invited_users":
    [
        "{user_id}"
    ],
    "mute_upon_entry": "DISABLE",
    "guest_dial_in": true,
    "watermark": false,
    "watermark_type": "single",
    "meeting_type": "recurrence",
    "recurrenceInterval": 2,
    "recurrenceStartTime": 1698215400000,
    "recurrenceEndTime": 1698219000000,
    "recurrenceStartDay": 1698215400000,
    "recurrenceEndDay": 1700927999000
}
```

**Response:**
```json
//http 200
{
    "meeting_type": "recurrence",
    "meeting_name": "Someone's meeting",
    "meeting_description": "XXXXXX",
    "meeting_number": "{meeting_number}",
    "call_rate_type": "2048K",
    "schedule_start_time": 1641525383726,
    "schedule_end_time": 1641532583788,
    "meeting_password": "111111",
    "invited_users_detail":
    [
        {
            "user_id": "{user_id}",
            "username": "test1",
            "real_name": "Test1"
        }
    ],
    "owner_name": "{owner_name}",
    "reservation_id": "{reservation_id}",
    "recurrence_gid": "{recurrence_gid}",
    "recurrence_type": "NONE",
    "meeting_url": "https://xxxxx/j/oP5idgfEmt0p"
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**

### Update a Recurrence Meeting 

**GET** `/api/v1/meeting_schedule/recurrence/{id}`

**Request:**
```json
{
    "recurrence_type": "DAILY",
    "meeting_description": "XXXXXX",
    "schedule_start_time": 1641525383726,
    "schedule_end_time": 1641532583788,
    "meeting_room_id": null,
    "call_rate_type": "2048K",
    "meeting_password": "111111",
    "invited_users":
    [
        "{user_id}"
    ],
    "mute_upon_entry": "DISABLE",
    "guest_dial_in": true,
    "watermark": false,
    "watermark_type": "single",
    "meeting_type": "recurrence",
    "recurrenceInterval": 2,
    "recurrenceStartTime": 1698215400000,
    "recurrenceEndTime": 1698219000000,
    "recurrenceStartDay": 1698215400000,
    "recurrenceEndDay": 1700927999000
}
```

**Response:**
```json
//http 200 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**

### Get Recurrence Meetings 

**GET** `/api/v1/meeting_schedule/recurrence/{group_id}`

**Query**  `page_num={number}&page_size={page_size}&filter={search_key}`

**Request:**


**Response:**
```json
//http 200
{
    "scheduled_meetings":
    [
        {
            "meeting_number": "12345678",
            "meeting_name": "scheduled meeting name",
            "meeting_type": "recurrence",
            "meeting_password": "{password}",
            "reservation_id": "{reservation_id}",
            "recurrence_gid": "{recurrence_gid}",
            "schedule_start_time": "{schedule_start_time}",
            "schedule_end_time": "{schedule_end_time}",
            "recurrence_type": "NONE",
            "owner_id": "{owner_id}",
            "owner_name": "{owner_name}",
            "meeting_url": "https://xxxxx/j/oP5idgfEmt0p"
        }
    ]
}

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**



## Meeting Control

### Mute All 

**POST** `/api/v1/meeting/{meeting_number}/mute_all`

**Request:**
```json
{
    "allow_self_unmute": true,
    "exclusions":
    [
        "$client_id"
    ]
}
```

**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**
| Field      | Type   | Description                        |
|------------|--------|------------------------------------|
| `allow_self_unmute`     | string | Updated name of the resource       |
| `exclusions`| long   | excluded the ids of client  |

**Response:**


### Unmute All 

**POST** `/api/v1/meeting/$meeting_number/unmute_all`

**Request:**


**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Mute One or More

**POST** `/api/v1/meeting/{meeting_number}/mute`

**Request:**
```json
{
    "allow_self_unmute": true,
    "participants":
    [
        "{client_id}"
    ]
} 
```

**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Unmute One or More 

**POST** `/api/v1/meeting/{meeting_number}/unmute`

**Request:**
```json
{
    "participants":
    [
        "{client_id}"
    ]
}
```

**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Stop Meeting

**POST** `/api/v1/meeting/{meeting_number}/stop`

**Request:**
```json
{
    "rejoin": true
}
```

**Response:**
```json
//http 200 or 204
empty

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Get Meeting List 

**POST** `/api/v1/meeting`

**Request:**

**Response:**
```json
//http 200
{
    "meetings":
    [
        {
            "meeting_number": "{meeting_number}",
            "meeting_name": "{meeting_name}",
            "start_time": 1698215400000,
            "status": "STARTED",
            "owner_id": "{user_id}",
            "owner_name": "{owner_name}",
            "locked": false
        }
    ]
}

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Get Meeting Detail 

**POST** `/api/v1/meeting/{meeting_number}`

**Request:**

**Response:**
```json
//http 200
{
    "meeting_number": "{meeting_number}",
    "meeting_name": "{meeting_name",
    "start_time": 1698219000000,
    "meeting_password": "{meeting_password}",
    "status": "STARTED",
    "owner_id": "{owner_id}",
    "owner_name": "{owner_name}",
    "guest_dial_in": true,
    "watermark": false,
    "locked": false,
    "participants":
    [
        {
            "client_uuid": "{client_id}",
            "name": "xxxx",
            "type": "GUEST",
            "mute_video": false,
            "mute_audio": false,
            "lecture": false
        }
    ]
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**



### Start Text Overlay 

**POST** `/api/v1/meeting/{meeting_number}/overlay`

**Request:**
```json
{
    "content": "Test Overlay Text!",
    "repeat": 5,
    "position": 100,
    "enable_scroll": true
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**



### Stop Text Overlay

**DELETE** `/api/v1/meeting/{meeting_number}/overlay`

**Request:**

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Set Lecturer 

**POST** `/api/v1/meeting/{meeting_number}/lecturer`

**Request:**
```json
{
    "lecturer": "{client_id}"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Unset Lecturer 

**DELETE** `/api/v1/meeting/{meeting_number}/lecturer`

**Request:**

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Disconnect One or More Participants 

**DELETE** `/api/v1/meeting/{meeting_number}/disconnect`

**Request:**
```json
{
    "participants":
    [
        "{client_id}"
    ]
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Disconnect All Participants 

**DELETE** `/api/v1/meeting/{meeting_number}/disconnect_all`

**Request:**
```json
{
    "participants":
    [
        "{client_id}"
    ]
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Cascading

**POST** `/api/v1/meeting/{meeting_number}/cascading`

**Request:**
```json
{
    "avc_call_params":
    [
        {
            "protocol": "H.323",
            "name": "RMXMeeting",
            "uri": "$IP##123",
            "call_rate": "4096"
        }
    ],
    "frtc_call_params":
    {
        "meeting_number": "123456",
        "password": "111111"
    }
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Update Active Meeting

**POST** `/api/v1/meeting/{meeting_number}`

**Request:**
```json
{
    "end_time": 1641532583788
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Update Participant Info in Meeting 

**POST** `/api/v1/meeting/{meeting_number}/participant`

**Request:**
```json
{
    "client_id": "{client_id}",
    "display_name": "Tom Cruise"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**



### Start Recording 

**POST** `/api/v1/meeting/{meeting_number}/recording`

**Request:**
```json
{
    "meeting_number": "123456"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Stop Recording

**DELETE** `/api/v1/meeting/{meeting_number}/recording`

**Request:**
```json
{
    "meeting_number": "123456"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Start Live Streaming 

**DELETE** `/api/v1/meeting/{meeting_number}/live`

**Request:**
```json
{
    "meeting_number": "123456",
    "live_password": "111111"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Stop Live Streaming 

**DELETE** `/api/v1/meeting/{meeting_number}/live`

**Request:**
```json
{
    "meeting_number": "123456"
}
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Pin for Participant 

**POST** `/api/v1/meeting/{meeting_number}/participant/{target_client_id}/pin`

**Request:**
```json
{
    "participants":
    [
        "client_id"
    ]
}

// *only for gateway client 
// *participants either can be set to one other participants or empty 
// *please use layout change api for layout control. 
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Unpin for Participant 

**DELETE** `/api/v1/meeting/{meeting_number}/participant/{pinned_client_id}/pin`

**Request:**
```json
{
    "participants":
    [
        "client_id"
    ]
}

// *only for gateway client 
// *participants either can be set to one other participants or empty 
// *please use layout change api for layout control. 
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Pin for Meeting 

**POST** `/api/v1/meeting/{meeting_number}/pin`

**Request:**
```json
{
    "participants":
    [
        "client_id"
    ]
}

// *only for ongoing meeting 
// *participants either can be set to other participants.  
// *currently, only one participant can be pinned for one meeting. 
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Unpin for Meeting 

**DELETE** `/api/v1/meeting/{meeting_number}/pin`

**Request:**
```json
{
    "participants":
    [
        "client_id"
    ]
}

```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Layout Change for Participant 

**POST** `/api/v1/meeting/{meeting_number}/participant/{viewer_client_id}/layout_change`

**Request:**
```json
{
    "max_views": 9,
    "layout": "auto"
}
// *max_views: 1、9 
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Request Unmute 

**POST** `/api/v1/meeting/$meeting_number/request_unmute`

**Request:**

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Allow Unmute 

**POST** `/api/v1/meeting/$meeting_number/allow_unmute`

**Request:**
```json
{
    "participants":
    [
        "$claimer_client_uuid"
    ]
}
```

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**


### Lock Meeting 

**POST** `/api/v1/meeting/$meeting_number/lock?`

**Request:**
```json
{
    "participants":
    [
        "$claimer_client_uuid"
    ]
}
```

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**

### Unlock Meeting 

**DELETE** `/api/v1/meeting/$meeting_number/lock?`

**Request:**
```json
{
    "participants":
    [
        "$claimer_client_uuid"
    ]
}
```

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Response:**



## User

### Sign In 

**POST** `/api/v1/user/sign_in`

**SignedIn**  `False`

**Request:**
```json
{
    "username": "xxxx",
    "secret": "xxxxx"
}
```

**Response:**
```json
//http 200 or 204
{
    "user_token": "xxxx",
    "user_id": "{user_id}",
    "username": "{username}",
    "email": "{email}",
    "real_name": "{realName}",
    "department": "{depart}",
    "mobile": "{mobile}",
    "password_expired_time": 60,
    "security_level": "HIGH",
    "role":
    [
        "Normal"
    ]
}

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**
| Field      | Type   | Description                        |
|------------|--------|------------------------------------|
| `user_token`       | int    | the token of user |


### Sign Out

**POST** `/api/v1/user/sign_out`

**Request:**

**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**



### User Info 

**POST** `/api/v1/user/info`

**Request:**

**Response:**
```json
//http 200 or 204
{
    "user_token": "xxxx",
    "user_id": "{user_id}",
    "username": "{username}",
    "email": "{email}",
    "real_name": "{realname}",
    "department": "{depart}",
    "mobile": "{mobile}",
    "password_expired_time": 60,
    "security_level": "HIGH",
    "role":
    [
        "Normal"
    ]
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**


### Update Password 

**POST** `/api/v1/user/password`

**Request:**
```json
{
    "secret_old": "xxxxx",
    "secret_new": "xxxxx"
}

// *secret_old:sha1(old_password+salt) 
// *secret_new:sha1(new_password+salt) 
```
**Response:**
```json
//http 200 or 204

//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**



### Find Users by Page 

**POST** `/api/v1/user/public/users`

**Query**  `page_num={number}&page_size={page_size}&filter={search_key}`

**SignedIn**  `False`

**Request:**

**Response:**
```json
//http 200 or 204
{
    "users":
    [
        {
            "user_id": "{user_id}",
            "username": "{username}",
            "real_name": "{real_name}"
        }
    ],
    "total_page_num": 1,
    "total_size": 10
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**



### Create User 

**POST** `/api/v1/user/users?`

**Request:**
```json
{
    "username": "{username}",
    "real_name": "{real_name}",
    "email": "{email}",
    "department": "{depart}",
    "mobile": "{mobile}",
    "role": "Normal",
    "secret": "secret_token"
}

// *available for SystemAdmin 
// *username is global unique 
// *role:Normal、MeetingOperator、SystemAdmin 
```
**Response:**
```json
//http 200 or 204
{
    "user_id": "{user_id}"
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**

### Update User 

**POST** `/api/v1/user/users/$user_id?`

**Request:**
```json
{
    "real_name": "{real_name}",
    "email": "{email}",
    "department": "{depart}",
    "mobile": "{mobile}",
    "role": "Normal",
    "secret": "secret_token"
}
// *available for SystemAdmin 
// *username is global unique 
// *role:Normal、MeetingOperator、SystemAdmin 
// *secret is optional. 
```
**Response:**
```json
//http 200 or 204
{
    "user_id": "{user_id}"
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**



## Meeting Room

### Query Meeting List 

**POST** `/api/v1/meeting_room`

**Request:**


**Response:**
```json
//http 200 or 204
{
    "meeting_rooms":
    [
        {
            "meeting_room_id": "{meeting_room_id}",
            "meeting_number": "{meeting_number}",
            "meetingroom_name": "{meetingroom_name}",
            "meeting_password": "{password}",
            "owner_id": "{owner_id}",
            "owner_name": "{owner_name}",
            "creator_id": "{creator_id}",
            "creator_name": "{creator_name}",
            "created_time": 1635425714772
        }
    ]
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**




## System

### Get System Capacity  

**POST** `/api/v1/system/capacity`

**Request:**


**Response:**
```json
//http 200 or 204
{
    "gateway":
    {
        "max_calls": 20,
        "current_calls": 0
    },
    "system":
    {
        "max_calls": 1000,
        "max_users": 50,
        "current_calls": 1,
        "current_users": 48,
        "status": "authorized"
    }
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**


### Get System Version 

**POST** `/api/v1/system/version`

**Request:**


**Response:**
```json
//http 200 or 204
{
    "version": "3.4.0-16888"
}
//other
{ 
"error": "{error_meesage}",
"errorCode": "{error_code}"
}
```

**Description:**

**Request:**

**Response:**
