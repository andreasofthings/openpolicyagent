# Managed OPA Client

Managed Open Policy Agent

This is an implementation that will integrate OPA with a centrally managed policy.

## Running the example

1. Running a 

1.a. Mac/Linux.

Run ./agent.sh from a terminal. On a corporate you may need to request admin priviledges beforehands. This is required, because OPA will open outbound connections.

1.b. Windows

No idea. Contributions are welcome.

2. You should see something along these lines

```
{"addrs":[":8181"],"insecure_addr":"","level":"info","msg":"First line of log stream.","time":"2019-08-07T11:06:50+02:00"}
{"level":"info","msg":"Starting bundle downloader.","name":"http-example","plugin":"bundle","time":"2019-08-07T11:06:50+02:00"}
{"level":"info","msg":"Starting decision log uploader.","plugin":"decision_logs","time":"2019-08-07T11:06:50+02:00"}
{"level":"info","msg":"Starting status reporter.","plugin":"status","time":"2019-08-07T11:06:50+02:00"}
{"level":"info","msg":"Log upload skipped.","plugin":"decision_logs","time":"2019-08-07T11:06:50+02:00"}
{"level":"info","msg":"Bundle downloaded and activated successfully.","name":"http-example","plugin":"bundle","time":"2019-08-07T11:06:51+02:00"}
{"level":"info","msg":"Status update sent successfully in response to bundle update.","plugin":"status","time":"2019-08-07T11:06:51+02:00"}
```

Since config.yaml specifies a polling period, successful download & activation messages should repeat about every 2 minutes.

3. OPA Will report successful activation

server-logs:
```
2019-08-07 09:40:25 default[20190807t112828]  "POST /authz-control-plane/v1/status HTTP/1.1" 200
2019-08-07 09:40:25 default[20190807t112828]  {'labels': {'app': 'myapp', 'environment': 'production', 'id': '0a8a87d1-9a78-439d-aaba-9d7cc000505b', 'region': 'west', 'version': '0.12.0'}, 'bundle': {'name': 'http-example', 'active_revision': '2019-08-07', 'last_successful_activation': '2019-08-07T09:40:25.312232Z', 'last_successful_download': '2019-08-07T09:40:25.304794Z'}}
```


4. Query for authorizations!

```
curl localhost:8181/v1/data/http/allow -i -d @alice.json -H 'Content-Type: application/json'
```

Should produce (because Alice is allowed):
```
HTTP/1.1 200 OK
Content-Type: application/json
Date: Wed, 27 Nov 2019 14:57:43 GMT
Content-Length: 68

{"decision_id":"304ad7e8-651f-4645-b177-def69f6cf0f4","result":true}
```
