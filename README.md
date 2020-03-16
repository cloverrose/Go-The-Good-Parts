# Go-The-Good-Parts

## time/rate Limitter

When our API depends on other API. And it has (publicly defined) rate limit.
It is better to avoid rate limit error from other API by using time/rate Limiter feature.

- [time/rate](https://github.com/golang/time/blob/555d28b269f0569763d25dbe1a237ae74c6bcc82/rate/rate.go#L252-L267)
- [Useage of time/rate/limitter.Wait](https://github.com/hashicorp/vault/blob/75461a65220bd85dc3913ecdf75f07bb108cccfb/api/client.go#L786-L788)

memo:
- limit should be > 0 (0 is not invalid but meaningless)
- burst should be > 0

## sync RWMutex
[How to lock nested field](https://github.com/hashicorp/vault/blob/75461a65220bd85dc3913ecdf75f07bb108cccfb/api/client.go#L479-L486)

In this example, Client has Config field and both have lock sync.RWMutex field.
Client's method needs to get Config's write lock. This idiom can be used.


## http retry, redirect
[redirect](https://github.com/hashicorp/vault/blob/75461a65220bd85dc3913ecdf75f07bb108cccfb/api/client.go#L865-L889)
- we have to check redirect does not make https to http, it is called protocol downgrade.
- we have to check how many times redirect allowed. (In this example 1 redirect is allowed.)

["github.com/hashicorp/go-retryablehttp"]("github.com/hashicorp/go-retryablehttp")
- I don't check it but it may be useful.
- [how to convert doamin specific request to retryablehttp request](https://github.com/hashicorp/vault/blob/75461a65220bd85dc3913ecdf75f07bb108cccfb/api/request.go#L88-L147)

## retry, renew timing
[use jitter to avoid simultaneously retry](https://github.com/hashicorp/vault/blob/d76c04c6b5d80cdad0191c9138b336a56efb97b7/api/lifetime_watcher.go#L366-L381)

- if input=0 then return 0
- if input >0 then return random value 80%-90% of input.
- this output is used for secret renewal/reflesh timing decision, so 90-110% is not good, 80-90% is OK and safe.


## http error
[io.Copy+ioutil.NopCloser](https://github.com/hashicorp/vault/blob/75461a65220bd85dc3913ecdf75f07bb108cccfb/api/response.go#L35-L43)

usefull idiom. copy whole body and close original response body, then set new body made by NopCloser.


## Code comment

### Deprecated annotation

```
// Deprecated
var YourVariableName int
```

Only just below one is affected.