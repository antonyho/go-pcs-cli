# go-pcs-cli
A Baidu PCS CLI tool written in Go

This is a command line tool for listing, uploading, downloading, deleting files on Baidu Cloud Storage(PCS).

[Overview SDK](http://developer.baidu.com/wiki/index.php?title=docs/pcs)

[Baidu OAuth 2.0](http://developer.baidu.com/wiki/index.php?title=docs/oauth)

[PCS OAuth 2.0](http://developer.baidu.com/wiki/index.php?title=docs/pcs/guide/token_authorize)

[REST](http://developer.baidu.com/wiki/index.php?title=docs/pcs/rest/file_data_apis_list)

### Dependencies
* [Go OAuth2](https://github.com/golang/oauth2)
* [Godep](https://github.com/tools/godep)


##### Demonstration on getting token with `curl`
Ref: [https://blog.lilydjwg.me/posts/44021](https://blog.lilydjwg.me/posts/44021)
```
$ BDUSER=<Baidu username>
$ PASS=<Baidu password>
```

Get the cookie from Baidu
`curl -b cookies -c cookies http://www.baidu.com/ -sS -o /dev/null`

Getting the token
```
$ TOKEN=$(curl -b cookies -c cookies -sS "https://passport.baidu.com/v2/api/?getapi&tpl=mn&apiver=v3&class=login&tt=$(date +%s860)&logintype=dialogLogin" | tr "'" '"' | jshon -e data -e token -u)
$ curl -b cookies -c cookies "https://passport.baidu.com/v2/api/?logincheck&token=$TOKEN&tpl=mn&apiver=v3&tt=$(date +%s)&username=$BDUSER&isphone=false"
{"errInfo":{ "no": "0" }, "data": { "codeString" : "", "vcodetype" : "" }}
```

Login
```
$ curl -b cookies -c cookies --compressed -sS 'https://passport.baidu.com/v2/api/?login' -H 'Content-Type: application/x-www-form-urlencoded' --data "staticpage=http%3A%2F%2Fpan.baidu.com%2Fres%2Fstatic%2Fthirdparty%2Fpass_v3_jump.html&charset=utf-8&token=$TOKEN&tpl=mn&apiver=v3&tt=$(date +%s083)&codestring=&safeflg=0&u=http%3A%2F%2Fpan.baidu.com%2F&isPhone=false&quick_user=0&logintype=basicLogin&username=$BDUSER&password=$PASS&verifycode=&mem_pass=on&ppui_logintime=57495&callback=parent.bd__pcbs__ax1ysj" | grep -F 'err_no=400032' > /dev/null
```
