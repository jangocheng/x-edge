# x-edge notes

record something for x-edge

## 使用web/service的框架结构，改造x-edge

问题：

+ 按照web/service的结构，micro.Service的位置：type service struct -> opts Options -> Service  micro.Service
    而edgeService的结构：

```go
    type edgeService struct {
        opts    service.Options
        service micro.Service
    }
```

+ 考虑给edgeService重新定义 Options

+ web/service中微服务Service  micro.Service，在newOptions时创建了，在web/service的Init函数中调用了micro.Service的Init，但是却并未run。
    原因：主要是web/service使用micro.Service的client，不需要run。

    但是对于x-edge来说micro.Service的server需要run起来，因为x-edge需要被其他服务调用，向通过tcp/udp向下发送命令，或者x-edge需要用broker向订阅段推送消息。
    这里需要考虑为x-edge启动micro.Service的server

    micro.web