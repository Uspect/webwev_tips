# Ошибки браузера


#### 1. Access to fetch at 'XXX' from origin 'YYY' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
а) Установить расширение для браузера (напр.: https://chrome.google.com/webstore/detail/allow-cors-access-control)

б) Включить CORS в настройках IDE. Для VS Code нужно изменить **runtimeArgs**:

	    {
	       "version": "0.2.0",
	         "configurations": [
	             {
	                 "name": "Chrome",
	                 "type": "chrome",
	                 "request": "launch",
	                 "url": "http://localhost:3000",
	                 "webRoot": "${workspaceRoot}/src",
	                 "runtimeArgs": ["--disable-web-security"]
	             }
	         ]
	     }

---
#### 2. Access to fetch at 'XXX' from origin 'YYY' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: It does not have HTTP ok status.
Для MacOS в терминале `open -a Google\ Chrome --args --disable-web-security --user-data-dir`
