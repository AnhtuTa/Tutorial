### 1. Reference source:
https://developer.openstack.org/api-guide/quick-start/index.html

### 2. Get token
- Đầu tiên, muốn gọi được các API từ openstack thì ta cần phải lấy token, sau đó với mỗi API, ta phải insert token vào header thì mới gọi được, nếu ko sẽ bị lỗi 401 (Unauthorized)
- Muốn lấy token thì tham khảo ở đây:  
https://developer.openstack.org/api-guide/quick-start/api-quick-start.html
- VD: giả sử server chạy trên IP: http://20.0.1.10, và URL của service identity là http://20.0.1.10/identity, thì ta có thể lấy token như sau:
```
    POST: http://20.0.1.10/identity/v3/auth/tokens
    Header: Content-Type=application/json
    Body:
    {
        "auth": {
            "identity": {
                "methods": [
                    "password"
                ],
                "password": {
                    "user": {
                        "domain": {
                            "name": "Default"
                        },
                        "name": "tu.ta1",
                        "password": "abc13579"
                    }
                }
            }
        }
    }
```
Response sau khi thực hiện request trên: server sẽ trả về 1 token nằm ở HEADER, có dạng
```
x-subject-token → gAAAAABb0gIQHcOfCHW868LIb2XLgZJxVJGZTHARv8saH5o8Z9OPqFcySBCeCAo3rhSj2XlVyAG-sIwEwi8h052b0XNQMnHwLZeO01C4cDKxZ2Y9LJtVtettEBVNJbAocsXnttRJDXRfWNUBWBEDSu1zX5XdQVL7cpa05V3UJDI6LwEOCAdakJw
```
Bây giờ có thể dùng token này để thực hiện việc gọi các API ở dưới đây

### 3. Call API
- Vẫn vào trang sau, ta sẽ thấy có rất nhiều loại API được hỗ trợ:  
https://developer.openstack.org/api-guide/quick-start/index.html
- Ở đây demo phần compute API:  
https://developer.openstack.org/api-ref/compute/

#### 3.1. Service URLs
- All API calls described throughout the rest of this document require authentication with the OpenStack Identity service. After authentication, a base service url can be extracted from the Identity token of type compute. This service url will be the root url that every API call uses to build a full path.
- For instance, if the service url is http://mycompute.pvt/compute/v2.1 then the full API call for /servers is http://mycompute.pvt/compute/v2.1/servers.
- Tạm hiểu là: VD: Giả sử service URL là: http://20.0.1.10/compute/v2 thì API để lấy thông tin các servers là http://20.0.1.10/compute/v2/servers  
- CHÚ Ý:  
Cần thêm X-Auth-Token ở header để gọi các API)  
KHÔNG được có dấu / ở cuối: như này http://20.0.1.10/compute/v2/servers/ thì sẽ ko gọi được API

#### 3.2. 1 số VD call API (từ trang chủ)
- tham khảo: https://developer.openstack.org/api-ref/compute/?expanded=#list-servers
- Lấy tất cả servers :  
GET: http://20.0.1.10/compute/v2/servers
- Show info của 1 server có ID = 874db140-9857-476e-aab8-09bf9be39b6e:  
GET: http://20.0.1.10/compute/v2/servers/874db140-9857-476e-aab8-09bf9be39b6e
- ...
- Insert 1 server:
```
POST: http://20.0.1.10/compute/v2/servers
Header: X-Auth-Token=...; Content-Type=application/json
Body:
{
    "server" : {
        "accessIPv4": "1.2.3.4",
        "accessIPv6": "80fe::",
        "name" : "This is att server :v",
        "imageRef" : "0cfb0534-65c1-4798-91ae-ee53ee6d65e7",
        "flavorRef" : "42",
        "availability_zone": "nova",
        "OS-DCF:diskConfig": "AUTO",
        "metadata" : {
            "My Server Name" : "Apache1"
        },
        "personality": [
            {
                "path": "/etc/banner.txt",
                "contents": "ICAgICAgDQoiQSBjbG91ZCBkb2VzIG5vdCBrbm93IHdoeSBp dCBtb3ZlcyBpbiBqdXN0IHN1Y2ggYSBkaXJlY3Rpb24gYW5k IGF0IHN1Y2ggYSBzcGVlZC4uLkl0IGZlZWxzIGFuIGltcHVs c2lvbi4uLnRoaXMgaXMgdGhlIHBsYWNlIHRvIGdvIG5vdy4g QnV0IHRoZSBza3kga25vd3MgdGhlIHJlYXNvbnMgYW5kIHRo ZSBwYXR0ZXJucyBiZWhpbmQgYWxsIGNsb3VkcywgYW5kIHlv dSB3aWxsIGtub3csIHRvbywgd2hlbiB5b3UgbGlmdCB5b3Vy c2VsZiBoaWdoIGVub3VnaCB0byBzZWUgYmV5b25kIGhvcml6 b25zLiINCg0KLVJpY2hhcmQgQmFjaA=="
            }
        ],
        "security_groups": [
            {
                "name": "default"
            }
        ],
        "user_data" : "IyEvYmluL2Jhc2gKL2Jpbi9zdQplY2hvICJJIGFtIGluIHlvdSEiCg=="
    },
    "OS-SCH-HNT:scheduler_hints": {
        "same_host": "48e6a9f6-30af-47e0-bc04-acaed113bb4e"
    }
}
```
Response là server đã được insert:
```
{
    "server": {
        "security_groups": [
            {
                "name": "default"
            }
        ],
        "OS-DCF:diskConfig": "AUTO",
        "id": "94bbbccb-4be5-4223-909d-2321cec74eb1",
        "links": [
            {
                "href": "http://20.0.1.10/compute/v2/servers/94bbbccb-4be5-4223-909d-2321cec74eb1",
                "rel": "self"
            },
            {
                "href": "http://20.0.1.10/compute/servers/94bbbccb-4be5-4223-909d-2321cec74eb1",
                "rel": "bookmark"
            }
        ],
        "adminPass": "ugNe52bRgntH"
    }
}
```
