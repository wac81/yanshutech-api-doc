
***所有的接口访问全部在/api/ 之下，以下所有接口请自动补全/api/。比如：wordFlag, 调用完整地址应该是http://host/api/wordFlag***


## 1.通过用户名密码得到access_token
### http example:
```
http POST http://host/api/token grant_type=password client_id=your client_id client_secret=your client_secret username=your email password=your password
```
### 请求路径 token/
### request body
```javascript
grant_type:password   <---don't change
client_id:your client_id
client_secret:your client_secret
username:your email
password:your password
```
### request：post body
### response:
```javascript
{
  "access_token": "03f9cf55e2df866be11754a99c816a6af2a36a351c9a6b190c262b7f1f3fd32c",
  "refresh_token": "3d1bc51df8c37b2f7a64f3f114ed34a101006743974b3dfa8f8d949dc5f765bb",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
### request example
[get access_token]: https://github.com/wac81/yanshutech-api-doc/blob/master/images/get_access_token_from_password.png "get access_token"
![](https://raw.githubusercontent.com/wac81/yanshutech-api-doc/master/images/get_access_token_from_password.png)

## 2.通过refresh_token 交换access_token
### http example:
```
http POST http://host/api/token grant_type=refresh_token client_id=name client_secret=abc123456 refresh_token=TOKEN
```
### 请求路径 token/
### request body
```javascript
grant_type:refresh_token        <---don't change
client_id:5ed21f01c3ff07912deb5bf042af9c32
client_secret:31066ed144d2ee16c0f74aa413ed8d02
refresh_token:64767236d6ac54433f092d3561043ee54a5a8a849a9b35a6c445e902e457b6eb
```
### request：post body
### response:
```javascript
{
  "access_token": "13c034aaf999fc94058f9239557962e0e398e0f401bafed5d2bc488c47ba401e",
  "refresh_token": "de5145538a03fd18676792d3d2d1bd5126dc111ada1ba867ecd459a8345bbdfa",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```
### request example
[get access_token]: https://github.com/wac81/yanshutech-api-doc/blob/master/images/get_access_token_from_refresh_token.png "get access_token"
![](https://raw.githubusercontent.com/wac81/yanshutech-api-doc/master/images/get_access_token_from_refresh_token.png)
