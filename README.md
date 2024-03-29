# Go Redis Test Hooks

[![go-redis-test-hook](https://github.com/mjc-gh/go-redis-test-hook/actions/workflows/tests.yaml/badge.svg)](https://github.com/mjc-gh/go-redis-test-hook/actions/workflows/tests.yaml)
[![Go Reference](https://pkg.go.dev/badge/github.com/mjc-gh/go-redis-test-hook.svg)](https://pkg.go.dev/github.com/mjc-gh/go-redis-test-hook)

Capture commands set to Redis for testing purposes with a
[hook](https://pkg.go.dev/github.com/go-redis/redis/v9#Client.AddHook).

```go
func TestDoesSomethingWithRedis(t *testing.T) {
    // Setup
    hook := redistesthooks.New()

    rdb := redis.NewClient(&redis.Options{Addr: "localhost:6379"})
    rdb.AddHook(hook)

    // Do stuff with redis
    rdb.Set(ctx, "key", 42, time.Duration(0))
    rdb.Get(ctx, "key")

    // Write assertions against captures
    assert.Equal(t, "SET key 42", hook.Captures[0].String())
    assert.Equal(t, "GET key", hook.Captures[1].String())

    // Clear captures
    hook.Reset()
}
```
