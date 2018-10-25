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
- Tạm hiểu là: VD: Giả sử service URL là: http://20.0.1.10/compute/v2 thì API để lấy thông tin các servers là http://20.0.1.10/compute/v2/servers (Chú ý: cần thêm X-Auth-Token ở header để gọi các API)
#### 3.2. 1 số VD call API (từ trang chủ)
- tham khảo: https://developer.openstack.org/api-ref/compute/?expanded=#list-servers
- Lấy tất cả servers :  
GET: http://20.0.1.10/compute/v2/servers
- Show info của 1 server có ID = 874db140-9857-476e-aab8-09bf9be39b6e:
GET: http://20.0.1.10/compute/v2/servers/874db140-9857-476e-aab8-09bf9be39b6e
- ...
