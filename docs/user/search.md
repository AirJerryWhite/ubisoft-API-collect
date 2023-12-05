# 查询用户账号信息

## 查询用户账号

> https://public-ubiservices.ubi.com/v2/profiles

*请求方法:GET*

**url参数：**

| 参数名            | 类型  | 内容   | 必要性 | 备注 |
|----------------|-----|------|-----|----|
| nameOnPlatform | str | 用户名称 | 必要  |    |
| platformType   | str | 平台Id | 必要  |    |

**json回复:**

| 参数名      | 类型   | 内容   | 备注           |
|----------|------|------|--------------|
| profiles | list | 用户档案 | 若用户不存在，则为空列表 |

`profiles`数组:

| 项              | 类型  | 内容   | 备注                                   |
|----------------|-----|------|--------------------------------------|
| profileId      | str | 档案Id |                                      |
| userId         | str | 用户Id |
| platformType   | str | 平台代码 | PC端：uplay<br />PSN：psn<br />XBOX：xbl |
| idOnPlatform   | str | 平台ID |                                      |
| nameOnPlatform | str | 平台名称 |                                      |

**示例：**

查询`PC端`玩家`AirJerryWhite`的用户档案。

```shell
curl -G 'https://public-ubiservices.ubi.com/v2/profiles' \
--data-urlencode 'nameOnPlatform=AirJerryWhite&platformType=uplay'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "profiles": [
    {
      "profileId": "0f18d06a-6490-4097-bfcf-4e16fac7cc95",
      "userId": "0f18d06a-6490-4097-bfcf-4e16fac7cc95",
      "platformType": "uplay",
      "idOnPlatform": "0f18d06a-6490-4097-bfcf-4e16fac7cc95",
      "nameOnPlatform": "AirJerryWhite"
    }
  ]
}
```

</details>

查询`PSN`玩家`CowGhostSnakeGod`的用户档案。

```shell
curl -G 'https://public-ubiservices.ubi.com/v2/profiles' \
--data-urlencode 'nameOnPlatform=CowGhostSnakeGod&platformType=psn'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "profiles": [
    {
      "profileId": "edf1f3b3-afe9-40e8-bdf6-4a67f41b6111",
      "userId": "91fac390-ea60-4aa3-b7e4-1476ddfb2530",
      "platformType": "psn",
      "idOnPlatform": "577119959882852399",
      "nameOnPlatform": "CowGhostSnakeGod"
    }
  ]
}
```

</details>

查询`XBOX`玩家`CowGhostSnakeGod`的用户档案。

```shell
curl -G 'https://public-ubiservices.ubi.com/v2/profiles' \
--data-urlencode 'nameOnPlatform=CowGhostSnakeGod&platformType=xbl'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "profiles": []
}
```

</details>
