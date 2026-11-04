# OneTimeSecret 懒猫微服包

安全分享密码与隐私链接：一次性网址，查看后失效。

- 上游：<https://github.com/onetimesecret/onetimesecret>
- 镜像：`onetimesecret/onetimesecret`
- 许可：MIT（全文见 content/licenses/LICENSE，文件名为 LICENSE.txt）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `onetimesecret` | `onetimesecret/onetimesecret` | 应用本体，监听 3000 |
| `maindb` | `valkey/valkey:8.1-bookworm` | 存待查看的密文与账号数据 |

## 关键配置说明

- **`SSL=true` 与 `SESSION_COOKIE_SECURE=true` 必须同时设置**：上游在 compose 注释里
  明确写到，这套栈本身只跑明文 HTTP，若前面有 TLS 终结器（本包就是），
  不打开这两个开关则**应用会拒绝写 session cookie，导致每次登录都失败**。
- **`SECRET` 与 `VALKEY_PASSWORD` 是必填项**：上游 compose 用 `:?` 断言，
  未设置会直接启动失败。本包分别用 stable_secret 生成。
- `VALKEY_URL` 里的密码与 maindb 的 `--requirepass` 用同一个 secret。
- 应用自带账号体系，因此开放 `public_path`，由应用自己鉴权。
- 未配置 SMTP：注册验证与找回密码需自行接邮件服务；
  不登录直接生成一次性链接不受影响。

## 未验证部分

未在真机上实测：注册登录、生成与查看密文、过期失效、重启后数据留存。
