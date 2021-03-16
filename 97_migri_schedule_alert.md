## How to monitor Migri available timeframes

Put that into infinite cycle with sleep in console:

```bash
# FIXME: Copy cURL code from browser (Chrome has it)
data=`
  curl -s 'https://migri.vihta.com/public/migri/api/scheduling/offices/25ee3bce-aec9-41a7-b920-74dc09112dd4/2021/w6?end_hours=24&start_hours=0' \
  -H 'authority: migri.vihta.com' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'dnt: 1' \
  -H 'vihta-session: YOUR-SESSION-ID-HERE' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 11_2_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36' \
  -H 'content-type: application/json;charset=UTF-8' \
  -H 'sec-gpc: 1' \
  -H 'origin: https://migri.vihta.com' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-dest: empty' \
  -H 'referer: https://migri.vihta.com/public/migri/' \
  -H 'accept-language: en-GB,en-US;q=0.9,en;q=0.8,ru;q=0.7,fi;q=0.6' \
  -H 'cookie: __cfduid=1234567' \
  --data-raw '{"serviceSelections":[{"values":["2906a690-4c8c-4276-bf2b-19b8cf2253f3"]}],"extraServices":[]}' \
  --compressed
`

result=`echo $data | jq '.dailyTimesByOffice | reduce .[] as $item ([]; . + $item) | isempty(.[])'`

if [[ "$result" == "false" ]]
then
  osascript -e 'display notification "There is free slot available" sound name "Submarine"'
  echo "https://migri.vihta.com/public/migri/#/reservation"
fi
```

```bash
bash -c 'while true; do bash ./migri-check.sh; sleep 900; done'
```
