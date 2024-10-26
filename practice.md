# practice

## describe requirements

1、monitor web site status

2、Capture the reasons for website exceptions when issues occur.

2、The fetch interval is 30 seconds

```bash
#!/bin/bash
function init() {
  ALL_REQUIRE_PARMS[2]='monitors'
  ALL_REQUIRE_PARMS[1]='interval'
  ALL_REQUIRE_PARMS[0]='times'
}

function showHelp() {
  # When fetching for the third time, all attempts encountered website issues. Then, show the reasons and which websites had issues
  echo '
      Require all of the following parameters

      --monitors "['"'"https://www.baidu.com','https://www.bing.com"'"']" 
          monitor baidu
          monitor bing

      --interval "60"
          search 60s one time

      --times "3"
          search 3 times and all the web site had issue then alert
    '
  exit
}


function checkParms() {
  local GIVE_PARM="$1"
  local MATCH_PARM="false"
  local UNSET_INDEX=""

  for ((i=0;i<${#ALL_REQUIRE_PARMS[@]};i++));do
    if [[ ${GIVE_PARM##*'-'} == ${ALL_REQUIRE_PARMS[$i]} ]]; then
      MATCH_PARM="true"
      UNSET_INDEX=$i
    fi
  done

  if [[ $MATCH_PARM == "true" ]]; then
    unset ALL_REQUIRE_PARMS[$UNSET_INDEX]
    ALL_REQUIRE_PARMS=(${ALL_REQUIRE_PARMS[@]})
  fi

}


function lastCheck() {
  if [[ ${#ALL_REQUIRE_PARMS[@]} == "0" ]]; then
    return
  fi
  for ((i=0;i<${#ALL_REQUIRE_PARMS[@]};i++));do
    echo "lost those parms " ${ALL_REQUIRE_PARMS[$i]}
  done
  showHelp
}

function dnsCheck() {
  local URL=$1
  FQDN=$(echo "$URL" | sed -E 's|https?://([^/]+).*|\1|')
  nslookup $FQDN -timeout=5 > /dev/null
  if [[ $? != "0" ]]; then
    echo "dnsError"
    return
  fi
  echo "true"
}



function curlCheck() {
  local URL=$1
  GET_CURL_RS=`curl -o /dev/null -s -w '%{time_total}:%{http_code}' --connect-timeout 2 $URL`
  TIME_TOTAL=`echo ${GET_CURL_RS}|awk -F ":" '{print $1}'`
  HTTP_CODE=`echo ${GET_CURL_RS}|awk -F ":" '{print $2}'`
  TMP_TIME_OUT=`echo $TIME_TOTAL| grep -E -o "[0-9]+\." | grep -E -o "[0-9]+"`
  if [ $TMP_TIME_OUT -gt 0 ]; then
    echo "httpRequestTimeOUT"
    return
  fi
  if [[ $httpCode != "200" ]]; then
    echo "httpErrorCode200"
    return
  fi
  echo "true"
}


function runCheck() {
  local INDEX=$1
  local URL=$2
  local TIMES=$3

  DNS_CHECK=$(dnsCheck "$URL")
  if [[ ${DNS_CHECK} != "true" ]]; then
    MONITOR_TARGET_STATUS[$1]=$(echo "${MONITOR_TARGET_STATUS[$1]}" $DNS_CHECK)

    return
  fi
  HTTP_CHECK=$(curlCheck "$URL")
  if [[ ${DNS_CHECK} != "true" ]]; then
    MONITOR_TARGET_STATUS[$1]=$(echo "${MONITOR_TARGET_STATUS[$1]}" $DNS_CHECK)
  fi
  MONITOR_TARGET_STATUS[$1]=$(echo "${MONITOR_TARGET_STATUS[$1]}" "true")
}

function initMonitorCheck() {
  local INDEX=0
  MONITOR_TARGET_STATUS=()
  while IFS= read -r URL; do
    MONITOR_TARGET_STATUS[$INDEX]=""
    INDEX=$((${INDEX}+1))
  done <<< "$1"
}


# check alert is match 3 times
function checkALERT() {
  local INDEX=$1
  local URL=$2
  local WARRS=(${MONITOR_TARGET_STATUS[$INDEX]})
  local CHECKTIME=$3
  # if ckeck time less than gave checktime then pass check
  if [ "${#WARRS[@]}" -lt "$CHECKTIME" ]; then
    return
  fi
  # if ckeck time greater than gave checktime then check
  for (( i=${#WARRS[@]}-1; i>=${#WARRS[@]}-$CHECKTIME; i-- )); do
    if [[ ${WARRS[$i]} == "" ]]; then
      continue
    fi
    if [[ ${WARRS[$i]} == "true" ]]; then
      return
    fi
  done <<< "$WARRS"

  echo 'had alert:' "URL: $URL", "alertThreeTimesReason:" ${WARRS[@]}
  MONITOR_TARGET_STATUS[$INDEX]=""

}




function monitorCheck() {
  local MONITORS=$1
  local INTERVAL=$2
  local TIMES=$3
  
  TEMP_MONITORS=$(echo "$MONITORS" | tr -d "[]'" | tr ',' '\n')
  # create a array  MONITOR_TARGET_STATUS use for save check status
  initMonitorCheck "$TEMP_MONITORS"
  # start monitor
  while true; do
    local INDEX=0
    while IFS= read -r URL; do
      # monitor and edit globle array MONITOR_TARGET_STATUS 
      runCheck $INDEX "$URL" "$TIMES"
      # checke MONITOR_TARGET_STATUS is reqiure alert
      checkALERT $INDEX "$URL" "$TIMES"
      INDEX=$((${INDEX}+1))
    done <<< "$TEMP_MONITORS"
    sleep $INTERVAL
  done
}

function monitorMain() {
  # echo "111"
  local MONITORS=""
  local INTERVAL=""
  local TIMES=""
  while [[ $# -gt 0 ]]; do
    if [[ ${1##*'-'} == "monitors" ]]; then
      MONITORS=$2
    fi
    if [[ ${1##*'-'} == "interval" ]]; then
      INTERVAL=$2
    fi
    if [[ ${1##*'-'} == "times" ]]; then
      TIMES=$2
    fi
    shift
    shift
  done
  monitorCheck "$MONITORS" "$INTERVAL" "$TIMES"
}


# main check
function checkMain() {
  if [[ $# == "0" ]]; then
    showHelp
  fi
  while [[ $# -gt 0 ]]; do
    local PARM="${1}"
    local VALUE="${2}"
    if [[ $PARM != "--"* ]] && [[ $VALUE == "" ]]; then
      showHelp
    fi
    checkParms "$PARM"
    shift
    shift
  done
  lastCheck
}


init
checkMain $@
monitorMain $@
```

then use that command to run check
`./monitors.sh --monitors "['https://www.google.com','https://www.bing111.com']"  --interval "60"  --times "3"`
