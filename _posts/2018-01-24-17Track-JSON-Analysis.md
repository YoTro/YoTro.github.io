---
title: 17track JSON Analysis
date: 2018-01-24 14:52:31
layout: post
tags: [JSON,logistic]
---
# The structure of JSON in 17track

It Contains the Tracking number and logistics information

 `z0` contains Latest logistics information

 `z1` has Chinese logistics information

 `z2` has English logistics information

```json
JSON
ret	1
msg	Ok
g	27dbe45fb3ac49799cb407353b030c18
dat	[…]
    0	{…}
    no	EV880444237CN
    delay	0
    yt	null
    track	{…}
            b	301
            c	2105
            e	35
            f	-1
            w1	3013
            w2	21051
            ln1	zh-CHS
            ln2	en
            is1	1
            is2	1
            ygt1	91
            ygt2	10426
            ylt1	2018-01-24 06:44:09
            ylt2	2018-01-24 06:44:19
            hs	308371504
            yt	null
            z0	{…}
            z1	[…]
            z2	[…]
            zex	{…}
              sa	true
```

You can find a transition about Tracking package in `z0:z`.

```json
z0:"{…}"
a	2018-01-23 00:00
b	null
c
d	NORFOLK, VA 23518
z	Redelivery Scheduled, The customer has requested that the Postal Service redeliver this item on January 25, 2018 in NORFOLK, VA 23518.

```
# 5 transit status

## in transit

![loading...](/images/JSON/17track/ETKTrack_JSON_0(in Transit).png)

## Delivered

![loading...](/images/JSON/17track/ETKTrack_JSON_1(Deliverd).png)

## Failure

![loading...](/images/JSON/17track/ETKTrack_JSON_3(Failure).png)

## Alert

![loading...](/images/JSON/17track/ETKTrack_JSON_2(Alert).png)

## Pick up

![loading...](/images/JSON/17track/ETKTrack_JSON_1(Deliverd).png)

## Not found

![loading...](/images/JSON/17track/null_track.png)

## Delayed transmission

IF it is Delayed transmission,it will send two json : one size is 123 byte,another is 2.17KB.

First `guid` is null string,the second has a string and JSON has fullfill logistics information

![loading...](/images/JSON/17track/XHR_JSON_2(Alert).png)
# !IMPORT params introduction

- **f** If delivered, the `f` is **The arrival time of the goods at the customer's address**

- **e**

| e     | Expression   |   f   |
| :---: |  :---:       | :---: |
| 10    |  in Transit  |  $-1$ |
| 30    |  Pick up     |  $-1$ |
| 35    |  Failure     |  $-1$ |
| 40    |  Delivered   |  DAYS |
| 50    |  Alert       |  $-1$ |
