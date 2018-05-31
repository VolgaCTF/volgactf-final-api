# Themis Finals API

This documents describes Themis Finals public APIs, namely Capsule API and Flag API.

## Prerequisites
1. You should know a contest checking system's IP address or FQDN. In this guide `10.0.0.2` will be used.
2. You should have a command-line utility capable of sending HTTP requests. For instance, [curl](http://curl.haxx.se) is a fine one.

## Capsule API
### Getting public key
```
$ curl http://10.0.0.2/api/capsule/v1/public_key
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE6O4HeeDG/p7CYoHrDh54SBV2RoYW
oOvajNCsb0tBWPC6VZK2jTFhwzShgAnkwkUvzZMMdDiSmHCZOm5x6KZ25Q==
-----END PUBLIC KEY-----
```

## Flag API
### Getting flag info

```
$ curl http://10.0.0.2/api/flag/v1/info/869d8f1008731701a2bab8fca8b971ba=
{"flag":"869d8f1008731701a2bab8fca8b971ba=","nbf":"2018-05-31T11:38:55+00:00","exp":"2018-05-31T11:43:55+00:00","round":10,"team":"Lorem","service":"Ipsum"}
```

**Responses**

| HTTP code | Response | Description |
|-----------|----------|-------------|
| 200 | `{"flag":"869d8f1008731701a2bab8fca8b971ba=","nbf":"2018-05-31T11:38:55+00:00","exp":"2018-05-31T11:43:55+00:00","round":10,"team":"Lorem","service":"Ipsum"}` | JSON with flag metadata |
| 403 | ERROR_ACCESS_DENIED | Request is made from a non-team network |
| 404 | ERROR_NOT_FOUND | The flag does not exist |
| 429 | ERROR_RATELIMIT | Rate limit exceeded |
| | ERROR_UNKNOWN | General error |

### Submitting a flag
```
$ curl http://10.0.0.2/api/flag/v1/submit -H 'Content-Type: text/plain' -d 'c457f2f062ddd353d67a4c05b43bfda9='
SUCCESS
```

**Responses**

| HTTP code | Response | Description |
|-----------|----------|-------------|
| 200 | SUCCESS | The submitted flag has been accepted |
| 403 | ERROR_ACCESS_DENIED | Request is made from a non-team network |
| 400 | ERROR_COMPETITION_NOT_STARTED | The competition has not started yet |
| 400 | ERROR_COMPETITION_PAUSED | The competition is paused |
| 400 | ERROR_COMPETITION_FINISHED | The competition has already finished |
| 400, 413 | ERROR_FLAG_INVALID | Submitted data has invalid format |
| 429 | ERROR_RATELIMIT | Rate limit exceeded |
| 400 | ERROR_FLAG_EXPIRED | The submitted flag has expired |
| 400 | ERROR_FLAG_YOUR_OWN | The submitted flag belongs to the attacking team themselves |
| 400 | ERROR_FLAG_SUBMITTED | The submitted flag has been earlier accepted |
| 400 | ERROR_FLAG_NOT_FOUND | The submitted flag does not exist |
| 400 | ERROR_SERVICE_STATE_INVALID | The attacking team's service is not up |

## License
MIT @ [Alexander Pyatkin](https://github.com/aspyatkin)