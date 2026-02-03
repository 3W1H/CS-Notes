```Mermaid
sequenceDiagram
    autonumber
    participant PC as PC浏览器
    participant Server as 服务端
    participant App as 手机App

    Note over PC, App: 阶段一：初始化
    PC->>Server: 请求二维码 (UUID)
    Server-->>PC: 返回图片 + UUID

    Note over PC, App: 阶段二：扫码 (App主动)
    PC->>Server: 轮询: 状态变了吗?
    Server-->>PC: 状态: WAITING
    
    App->>Server: 手机扫码 (发送UUID)
    Server-->>App: 扫码成功
    Server->>Server: 状态更新 -> SCANNED

    PC->>Server: 轮询: 状态变了吗?`
    Server-->>PC: 状态: SCANNED
    PC->>PC: UI变更: "请在手机确认"

    Note over PC, App: 阶段三：确认 (App主动)
    App->>Server: 点击"确认登录"
    Server-->>App: 操作成功
    Server->>Server: 状态更新 -> CONFIRMED

    PC->>Server: 轮询: 状态变了吗?
    Server-->>PC: 状态: CONFIRMED (给Token)
    PC->>PC: 登录成功，跳转
```
