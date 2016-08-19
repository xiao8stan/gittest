一开始加密解密总是出错,结果是正则写错了,纠结了好久,浪费了很多时间.匹配任意字符的正则,下次就没问题啦
  
  
  ```
  url(r'^token/(?P<token>.+)',views.TokenLoginView.as_view(),name='token_login')
  ```
