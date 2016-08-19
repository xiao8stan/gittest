>1. 一开始加密解密总是出错,结果是正则写错了,纠结了好久,浪费了很多时间.匹配任意字符的正则,下次就没问题啦
  
  
  ```
  url(r'^token/(?P<token>.+)',views.TokenLoginView.as_view(),name='token_login')
  ```
>2. 加密算法用的是Crypto

 ```
  from Crypto import Random
  from Crypto.Cipher import AES
  BS = 16
  pad = lambda s: s + (BS - len(s) % BS) * chr(BS - len(s) % BS)
  unpad = lambda s: s[:-ord(s[len(s) - 1:])]
  keypad = lambda s: pad(s)[:16]


  class AESCipher:
      def __init__(self, key):
          self.key = keypad(key)
  
      def encrypt(self, raw):
          raw = pad(raw)
          iv = Random.new().read(AES.block_size)
          cipher = AES.new(self.key, AES.MODE_CBC, iv)
          return base64.b64encode(iv + cipher.encrypt(raw))
  
      def decrypt(self, enc):
          enc = base64.b64decode(enc)
          iv = enc[:16]
          cipher = AES.new(self.key, AES.MODE_CBC, iv)
          return unpad(cipher.decrypt(enc[16:]))
```
