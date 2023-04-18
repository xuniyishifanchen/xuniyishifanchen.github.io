---
title: "Download"
date: 2023-04-18T23:26:28+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "script"]
categories: ["shell script"]
---

# 断点续传下载脚本

## dl.sh
```shell
dl url
```

```shell
#!/bin/bash

USER_NAME="name"
USER_PWD="passwd."

if [ $# == 1 ] ; then
    URL=$1
    echo "URL is $URL"
    NEW_URL="${URL//https\:\/\//https\:\/\/$USER_NAME\:$USER_PWD@}"
    echo "NEW_URL is $NEW_URL"
    echo "Start download !!!!!"
    axel -aN6 -n 50 $NEW_URL
else
    echo "Please input download url!"
fi
```

## curl.py

```shell
./curl.py --jobs 8 --url downloadLink

```
```python
#!/usr/bin/python3
#-*-coding:utf-8-*

import os
import sys
import copy
import time
import argparse
import datetime
import subprocess as sp
from multiprocessing import Process


username = "name"
password = "passwd"
DOWNLOAD_DIR = os.path.dirname(os.path.abspath(__file__))
BACKUP_DIR = os.path.join(DOWNLOAD_DIR, ".backup")
DOWNLOAD_SUCCESS = 0
DOWNLOAD_FAIL = 1
DOWNLOAD_WAIT = 2

def shell(cmd):
    try:
        sp.call(cmd, shell=True)
    except KeyboardInterrupt:
        sys.exit(1)

def shout(cmd):
    return sp.check_output(cmd, shell=True)

def error_log(msg):
    cmd = 'echo "\033[31m%s\033[0m"' % msg
    shell(cmd)

def warning_log(msg):
    cmd = 'echo "\033[33m%s\033[0m"' % msg
    shell(cmd)

def info_log(msg):
    cmd = 'echo "\033[32m%s\033[0m"' % msg
    shell(cmd)

def info_begin():
    msg = ("############ Begin: %s ############"
           % sys._getframe().f_back.f_code.co_name)
    info_log(msg)

def info_end():
    msg = ("############ Finish: %s ############"
           % sys._getframe().f_back.f_code.co_name)
    info_log(msg)

def sec_ts():
    return datetime.datetime.now().timestamp()

def str_center(s, cnt):
    jl = int(cnt - len(s))
    rl = int(jl/2)
    return "%s%s%s" % (" "*(jl-rl), s, " "*rl) if jl > 0 else s

def bytes_conversion(bytes_cnt):
    units = ["KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB", "BB", "NB", "DB"]
    prefix = dict()
    for i,s in enumerate(units):
        prefix[s] = 1<<(i+1) * 10
    for s in reversed(units):
        if int(bytes_cnt) >= prefix[s]:
            value = float(bytes_cnt) / prefix[s]
            v_str = str(value)[:3]
            v_str = v_str[:-1] if v_str.endswith(".") else v_str
            return "%s%s" % (v_str, s)
    return "%dB" % bytes_cnt

def get_header_information():
    info_begin()
    cmd = "curl %s %s -I %s" % (LOGIN_STR, PROXY_STR, URL)
    res = shout(cmd).decode("utf-8")
    resume_enable = None
    for line in res.split("\n"):
        if line.startswith("Accept-Ranges"):
            resume_enable = True if line.split(":")[-1].strip() == "bytes" else False
        elif line.startswith("Content-Length"):
            total_bytes = int(line.split(":")[-1].strip())
    if resume_enable is None:
        error_log("Incorrect password or Invalid url: %s" % URL)
        sys.exit(1)
    info_end()
    return resume_enable, total_bytes

def calculate_each_block_size(total_bytes, bak_pg):
    info_begin()
    dest_name = os.path.basename(URL)
    task_list = []
    bytes_end = -1
    cnt = 1
    while True:
        bytes_start = bytes_end + 1
        if bytes_start >= total_bytes:
                break
        bytes_end = int(total_bytes * bak_pg * cnt / 100)
        bytes_end = (total_bytes - 1) if bytes_end > (total_bytes - 1) else bytes_end
        task_str = "-H 'Range: bytes=%d-%d'" % (bytes_start, bytes_end)
        cur_dic = {
            "bytes_start" : bytes_start,
            "bytes_end" : bytes_end,
            "check_size" : (bytes_end - bytes_start + 1),
            "task_str" : task_str,
            "task_idx" : cnt,
            "dest" : dest_name + "_part%d" % cnt
        }
        task_list.append(cur_dic)
        cnt += 1
    info_end()
    return task_list

def check_exist(task_list):
    info_begin()
    res = []
    for task in copy.deepcopy(task_list):
        dest = os.path.join(MULTI_DIR, task["dest"])
        if os.path.isfile(dest):
            if task["check_size"] == os.path.getsize(dest):
                continue
        res.append(task)
    info_end()
    return res

def download_by_multi_jobs(total_bytes, bak_pg, jobs):
    info_begin()
    WAIT_MAX_TIMES = 5
    speed_str_len = 8 # " xxxKB/s"
    percent_str_len = 7 # " 100.0%"
    dest_name = os.path.basename(URL)
    cmd = "mkdir -p %s" % MULTI_DIR
    shell(cmd)
    digit = max(len(str(total_bytes))+speed_str_len+1, len(" Process %d |" % jobs))

    all_task_list = calculate_each_block_size(total_bytes, bak_pg)
    dest_list = []
    for task in all_task_list:
        dest_list.append(os.path.join(MULTI_DIR, task["dest"]))

    # Check exist
    total_parts = len(all_task_list)
    task_list = check_exist(all_task_list)
    cur_parts = len(task_list)
    download_parts = total_parts - cur_parts
    if download_parts > 0:
        info_log("Exist %d parts, resuming." % download_parts)

    # log begin
    line = ""
    for i in range(jobs):
        idx = i + 1
        line += str_center("Process %d" % idx, digit) + "|"
    line += str_center("Total", speed_str_len + percent_str_len) + "|"
    print(line)

    # For log
    def log(last_len):
        print(last_len * "\b", end='')
        line = ""
        total_speed = 0
        for i, task in enumerate(work_jobs):
            idx = i + 1
            if task is None:
                line += str_center("- -", digit) + "|"
                continue
            last_size = task["last_size"]
            last_sec = task["last_sec"]
            sec = sec_ts()
            if os.path.isfile(task["dest"]):
                size = os.path.getsize(task["dest"])
                size_str = "%s" % size
            else:
                size = 0
                size_str = "%s" % "-"
            task["last_sec"] = sec
            task["last_size"] = size
            speed = 0 if sec == last_sec else (size - last_size)/(sec - last_sec)
            if speed < 0:
                speed = 0
                speed_str = task["last_speed_str"]
            else:
                speed_str = bytes_conversion(speed) + "/s"
                task["last_speed_str"] = speed_str
            line += str_center("%s %s" % (size_str, speed_str), digit) + "|"
            total_speed += speed
        total_pg = (download_parts * 100.0 / total_parts)
        total_str = "%.1f%s %s/s" % (total_pg, "%", bytes_conversion(total_speed))
        line += str_center(total_str, percent_str_len + speed_str_len) + "|"
        line += " " * 5
        print(line, end="")
        sys.stdout.flush()
        return len(line)

    # Multi process
    global work_jobs
    work_jobs = [None for i in range(jobs)]
    free_jobs = list(range(jobs))
    last_len = 0
    while True:
        # Download completed
        if download_parts == total_parts:
            print()
            break
        elif download_parts > total_parts:
            error_log("Something error happened. %d" % download_parts) 
            sys.exit(3)

        # Create and start processes
        while len(free_jobs) > 0 and task_list:
            task = task_list.pop(0)
            cmd = ("curl -s %s %s %s -o %s %s" %
                   (LOGIN_STR, PROXY_STR, task["task_str"], task["dest"], URL))
            cur_p = Process(target=shell, kwargs={'cmd':cmd}, daemon=True)
            job_idx = free_jobs.pop(0)
            task["process"] = cur_p
            work_jobs[job_idx] = task
            task["last_sec"] = sec_ts()
            task["last_size"] = 0
            task["last_speed_str"] = "0B/s"
            if not cur_p.is_alive():
                cur_p.start()

        last_len = log(last_len)
        time.sleep(0.5)

        # If process finish
        for idx, task in enumerate(work_jobs):
            res = DOWNLOAD_SUCCESS
            if task is not None and not task["process"].is_alive():
                if idx in free_jobs:
                    continue
                try:
                    size = os.path.getsize(task["dest"])
                    if size == task["check_size"]:
                        res = DOWNLOAD_SUCCESS
                    elif size == 0:
                        res = DOWNLOAD_WAIT
                    else:
                        res = DOWNLOAD_FAIL
                except:
                    res = DOWNLOAD_WAIT

                # If DOWNLOAD_SUCCESS, move to MULTI_DIR
                # If DOWNLOAD_FAIL, delete tmp file and insert the task to task_list
                # If DOWNLOAD_WAIT, do nothing but make wait_times +1 and
                #  ensure wait_times <= WAIT_MAX_TIMES
                if res == DOWNLOAD_WAIT:
                    if "wait_times" in task:
                        if task["wait_times"] > WAIT_MAX_TIMES:
                            res = DOWNLOAD_FAIL
                        else:
                            task["wait_times"] += 1
                    else:
                        task["wait_times"] = 1
                if res == DOWNLOAD_SUCCESS:
                    cmd = "mv %s %s" % (task["dest"], MULTI_DIR)
                    shell(cmd)
                    download_parts += 1
                    free_jobs.append(idx)
                elif res == DOWNLOAD_FAIL:
                    # process task
                    #print("断点续传测试")
                    #cur_size = os.path.getsize(task["dest"])
                    #cur_start = task["bytes_start"] + cur_size
                    #task["task_str"] = ("-C %s -H 'Range: bytes=%d-%d'"
                    #    % (cur_start, cur_start, task["bytes_end"]))
                    task_list.insert(0, task)
                    cmd = "rm -f %s" % task["dest"]
                    shell(cmd)
                    free_jobs.append(idx)
        last_len = log(last_len)
        time.sleep(0.5)

    # Merge file
    cmd = "rm -f %s" % dest_name
    shout(cmd)
    for item in dest_list:
        cmd = "cat %s >> %s" % (item, dest_name)
        shout(cmd)

    # Deleta backup
    for item in dest_list:
        cmd = "rm %s" % (item)
        #shell(cmd)
    info_end()

def download_directly(total_bytes):
    info_begin()
    cmd = "curl %s %s -O %s" % (LOGIN_STR, PROXY_STR, URL)
    shell(cmd)
    dest_name = os.path.basename(URL)
    check_size = os.path.getsize(dest_name)
    if check_size == total_bytes:
        info_log("Download successfully.")
    else:
        error_log("Download failed.")
    info_end()

def main(argv):
    parser = argparse.ArgumentParser(
        description=("A script for DOWNLOADING by RESUMING."))
    required_group = parser.add_argument_group(title="Required options")
    required_group.add_argument(
        '--url', dest="url", required=True, default="",
        help='Set url to download.')

    common_group = parser.add_argument_group(title="Common options")
    common_group.add_argument(
        "--back-up", dest="backup_progress", type=int, default=1,
        help="Set per progress to backup, which range from 1 to 100. Default: 1")
    common_group.add_argument(
        "--user", dest="username", type=str, default=username,
        help="Set username to download. Default: %s" % username)
    common_group.add_argument(
        "--pwd", dest="password", type=str, default=password,
        help="Set password to download. Default: %s" % password)
    common_group.add_argument(
        "--login", dest="login", action="store_true", default=False,
        help='Whether login to download. Defualt: False')
    common_group.add_argument(
        "--proxy", dest="proxy", type=str, default="",
        help='Set proxy to download. Default: ""')
    common_group.add_argument(
        "--jobs", dest="jobs", type=int, default=1,
        help='Set jobs to download. Default: 1')

    arg = parser.parse_args()

    global URL, LOGIN_STR, PROXY_STR
    URL = arg.url
    bak_pg = arg.backup_progress
    user = arg.username
    pwd = arg.password
    is_login_needed = arg.login
    proxy = arg.proxy
    jobs = arg.jobs
    LOGIN_STR = "-u %s:%s" % (user, pwd)
    PROXY_STR = "-x '%s'" % proxy

    start_ts = sec_ts()
    resume_enable, total_bytes = get_header_information()
    try:
        if resume_enable:
            global MULTI_DIR
            MULTI_DIR = os.path.join(BACKUP_DIR, "per%s" % bak_pg)
            try:
                download_by_multi_jobs(total_bytes, bak_pg, jobs)
            except KeyboardInterrupt:
                for task in work_jobs:
                    if task is not None:
                        process = task["process"]
                        if process.is_alive:
                            process.terminate()
                            process.join()
                raise KeyboardInterrupt()
            except Exception as error:
                for task in work_jobs:
                    if task is not None:
                        process = task["process"]
                        if process.is_alive:
                            process.terminate()
                            process.join()
                raise error
        else:
            warning_log("不支持断点续传")
            download_directly(total_bytes)
    except KeyboardInterrupt:
        print()
        warning_log("KeyboardInterrupt")
        sys.exit(1)
    end_ts = sec_ts()

    info_log("#### Download Successfully! (%d seconds) ####" % (end_ts-start_ts))

if __name__ == "__main__":
    main(sys.argv)


```