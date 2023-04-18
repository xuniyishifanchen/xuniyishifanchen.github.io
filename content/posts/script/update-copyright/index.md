---
title: "Update Copyright"
date: 2023-04-18T23:28:03+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "script"]
categories: ["shell script"]
---

# 更新copyright 脚本

```shell
#!/bin/bash

RED='\E[1;31m'     
GREEN='\E[1;32m'    
NOCOLOR='\e[0m'  

MODIFIED_FILES=$(git diff --name-only)
CURRENT_YEAR=$(date +%Y)

for FILE in $MODIFIED_FILES
do
    copyright=$(head -n 9 $FILE | grep "Copyright"|sed "s/\*//g")
    # echo $copyright
    if [[ $copyright =~ ([[:digit:]]+),[[:space:]]?([[:digit:]]+),[[:space:]]?([[:digit:]]+)[[:space:]]Healer[[:space:]]Corporation.
        || $copyright =~ ([[:digit:]]+),[[:space:]]?([[:digit:]]+)[[:space:]]Healer[[:space:]]Corporation.
        || $copyright =~ ([[:digit:]]+)[[:space:]]Healer[[:space:]]Corporation. ]]; the
        # echo ${BASH_REMATCH[-1]}
        if [[ ${BASH_REMATCH[-1]} != ${CURRENT_YEAR} ]]; then
            sed -i "s/[[:space:]]Healer Corporation./, $CURRENT_YEAR Healer Corporation./" $FILE
        fi
    else
        FILES_NEED_MANUAL_UPDATE="$FILE $FILES_NEED_MANUAL_UPDATE"
    fi
done

if [[ $FILES_NEED_MANUAL_UPDATE ]]; then 
    echo -e "${GREEN}Please manually update the copyright of the following files: ${NOCOLOR}"
    for FILE in $FILES_NEED_MANUAL_UPDATE
    do
        echo -e "${RED}${FILE}${NOCOLOR}"
    done 
fi
```

```java
Copyright "$today.year" Healer Corporation.
```