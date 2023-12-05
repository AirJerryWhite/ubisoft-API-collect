# 密码登陆

## 登陆流程

```python
账号 = 'Mika@teaparty.edu.com'
密码字符串 = 'password'

# 1.账号加密步骤
加密后的账号字符串 = base64编码(账号:密码字符串)

# 2.获取凭证
Authorization = 获取凭证(header={'Authorization': 'Basic 加密后的账号字符串'})
```

## 获取凭证

> https://public-ubiservices.ubi.com/v3/profiles/sessions

*请求方式：POST*

**header参数：**

| 参数名                       | 类型  | 内容               | 必要性 | 备注 |
|---------------------------|-----|------------------|-----|----|
| Ubi-RequestedPlatformType | str | 平台Id             | 必要  |    |
| Authorization             | str | 授权码              | 必要  |    |
| Ubi-AppId                 | str | （猜测是客户端类型）       | 必要  |    |
| Content-Type              | str | application/json |   |    |

`Ubi-RequestedPlatformType`字段：

| 平台   | 代码    |
|------|-------|
| PC   | uplay |
| XBOX | xbl   |
| PSN  | psn   |

`Authorization`字段：

加密后的账号字符串：

> Basic 加密后的账号字符串

`Ubi-AppId`字段：

| 客户端 | 代码                                   |
|-----|--------------------------------------|
| web | 880650b9-35a5-4480-8f32-6a328eaa3aad |

**json回复：**

| 字段                            | 类型                 | 内容         | 备注 |
|-------------------------------|--------------------|------------|----|
| platformType                  | str                | 平台         |    |
| ticket                        | str                | 凭证         |    |
| twoFactorAuthenticationTicket | <br />未开启二步验证：null | 数据本体       |
| profileId                     | str                | 档案Id       |    |
| userId                        | str                | 用户Id       |    |
| nameOnPlatform                | str                | 用户于当前平台的名称 |    |
| environment                   | str                |            |    |
| expiration                    | str                | 时间         |    |
| spaceId                       | str                |            |    |
| clientIp                      | str                | 客户端IP地址    |
| clientIpCountry               | str                | 客户端所在国家简写  |
| serverTime                    | str                | 服务器时间      |
| sessionId                     | str                | 会话Id       |
| sessionKey                    | str                |            |
| rememberMeTicket              | <br />未启用记住密码：null |            |

*后续请求接口时将请求头中的`Authorization`更改为 `Ubi_v1 t={ticket}`,`ticket`为json回复中的`ticket`字段的值。*

***凭证持续时间待测***

**示例：**

例如用户账号为`Mika@teaparty.edu.com`，密码为`password`
，Base加密后的账号字符串为`TWlrYUB0ZWFwYXJ0eS5lZHUuY29tOnBhc3N3b3Jk`，然后获取凭证。

```shell
curl 'https://public-ubiservices.ubi.com/v3/profiles/sessions' \
-X POST
--data-urlencode 'Ubi-RequestedPlatformType=uplay' \
--data-urlencode 'Authorization=Basic TWlrYUB0ZWFwYXJ0eS5lZHUuY29tOnBhc3N3b3Jk' \
--data-urlencode 'Ubi-AppId=880650b9-35a5-4480-8f32-6a328eaa3aad' \
--data-urlencode 'Content-Type=application/json'
```

<details>
<summary>查看响应示例：</summary>

```json
{
    "platformType": "uplay",
    "ticket": "**************************YTMyOGVhYTNhYWQiLAogICJlbnYiOiAiUHJvZCIsCiAgInNpZCI6ICJkZTk0YTkyOC0xOTJlLTRjYTYtOTY5ZC00N2NkN2Y0MzI2MjIiLAogICJ0eXAiOiAiSldFIiwKICAiZW5jIjogIkExMjhDQkMiLAogICJpdiI6ICJBcWhUTjNlUkJxRjNtaFdEUHJ0cTVnIiwKICAiaW50IjogIkhTMjU2IiwKICAia2lkIjogIjE5YzYyZWU4LWYwZjYtNGQxYi04MGIzLTQ1YWY2NGEyNjNiMiIKfQ.kKZV_5G1s4ajfX52FWGrb5gv_qnaXeXctdm_fLjA4F353bIh1Pin_tTfzolTp5G-MmKcHaHk4saW-3Nrw4qNyWoCUaE34UIN3PEgpN022LTpRVHNLpqsv8d133Rt3kh2ElLQn0vRdDcfMevC3CcLQeRtIZxsxr9VxNt_d2oJr6pcNebMiiap_z8GKNi8Vm784xsJ2KKDchA00Y7-5VDfm1E-tE_RcEZzKyrVjH2zB9hIytVp12sPSagXcwjB0Hg9Y8k2E0qzOaGgcSbhQshfZFY1lMIYhGV1bjfLmXnN1FdWnjJwj2rM8SbyyfcZ6NVXGYKpC4GEL1zfVTW_FRiJs6plYCUtEXO_otaVG4bK6d4CtUNZpDDf3Mvcgb-gtSzZyIHIDZJ9ifClFSIVimjw2dhlnzFSmr-KrCTKgXlopyONJNL-PkKGmzLQ8-2w13cewZ-kB2f9AQPj68WoqrdRIpFDdaEXSKSJfKNJiTWjsVRntI6JbVkW1fc9RsaWpPWca3h4fMyqBwtMCd9TSdh7HW5oE3UFtM_WZSNad0mRvstRaHOlPpRiPNEmkATWb7iWi6xc2PU2Tp8yosMOciFJFGp0Z7BP-HjpxtKgRzzqCkD5cgvw0QprvBXC4J28JrlG0tVFTTrr_imBsbKDM1OWxorXReGH1KuyINcpm8zCMtIUO2dNfjLiib-UR1s-o-jjc6dy72HaONFcI-ETuVhtiafkvGLKFSbAzuEf7LTOqJWtY-3BKIUrkzU4ocXN88d_BgYskm5--pkC7kltZ8XdzPbF0IuhQCYp56sAMISLfSfjG96j6KJN55TlJ9KGFtfC6gnUB9SybFHQzDwu7YYM3ZmauTZMODtxRj1WJ0pca1aYiEutjGrljcpLYdt6ZtmQLAlN5AVJER_as5uHfVWtYXXR_I0NziiiJBH1tY9jOicH_x8Q42XarKrkq4xJn5c3ghqV_oB7UnMUVgX1fKAl1HP6NZuc5Xf6KJ4SibBJ5h9k91Kg5cNWrJ3DKHBBAVbwWtssT4LhJWQiQcZ1IYz6NqfJEbGm6NSBvs1gHhPXLMT5auxJWENYolt_Zro9m5byFP-Wex4Tm2ewN9lBGt8-36ryhHJWksG8sY2fDAKhwbSpidf2VP63zsuR3yX7tT3f53VJVjseYZO6Yyzexe3mh2d8pJBrhzUfQdLZwKzefbFIcDSNNyTeg-MhcoZ3F_a2t5t-Zt-9Yvrkr41WJhRccwuVTpWa31RmOwHerUB0HmDIKeqivWclCWYTM8fuaMX9va9JlWfkdEaHak6FW0wBkPOxGJGtUGRiF4lkczKCDB_nVKk5hMh1ky6Pv5a9gcfsPRHrOH_tZ9i3GAciTmsIIFIp20oZ80ZhBWynfauYhRLf-oAeBFXua1OdPSv_qu2IcbC_fDr0gkN43K6jJuc0nZva3gv7s_pWW_zM0TtvcvvmQSAVF9QMdv5rc3qOKzlhzpPJjLGFXvQgkQqGJeIFmkBe14QsQvG-CAomoGZrfHJRVFoo2hLtdQiQcP_M-KEx8qt_nHi4jY2IE0wrYiV928JOGZBmJzNXajlP9M7W28QzHX1IFNXkLJWjWaQ9N05Xn2Lm7CGdHDqej5iLuACJbILn8Xhubm0pTQj1U8hWLo1S_AcqBiqIRzcPc0TLyG1Kidh_DeTIoD3t4Zo-aj2UqO5CFveP9VpeRVtPHlrw1ZmBdR_HX89siwX857rDV8px7SjTPIfKfdit_RvUA-pyS9OIOAx__aD1SdXAGogdRjWVQU_ywyDHLU55q-JANnGeAqShX8DyV2LnbU4dTUwOGcaHLs4SErWzUpXv4AAK8sfaK1rnD8xPz54fh6DbTyGlHiJQQUqKUwH4u-lwCYJA7RNXgzyz1Bnldpc7aanPydY.osGT9mftMpAXRv3tY5RH8kpvtMVq4oPRTLTJYQCr1xw",
    "twoFactorAuthenticationTicket": null,
    "profileId": "********-****-****-****-ecbbc8253184",
    "userId": "********-****-****-****-ecbbc8253184",
    "nameOnPlatform": "Mika",
    "environment": "Prod",
    "expiration": "2023-12-05T18:42:09.1632972Z",
    "spaceId": "3010fe4f-99f9-4cdb-a982-db92274cac01",
    "clientIp": "*.*.*.*",
    "clientIpCountry": "Kivotos",
    "serverTime": "2023-12-05T15:42:09.1808411Z",
    "sessionId": "********-****-****-****-47cd7f432622",
    "sessionKey": "*******************Dm56q2920bf/gZAYaq+YEAxUpPK4r73N9n6B9xyeNlJsA==",
    "rememberMeTicket": null
}

```

</details>