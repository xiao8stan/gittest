>1. 这里面的样式总是记不住,下次用直接来github上查一下倒是方便

```
{% if messages %}
            {% for message in messages %}
                <div class="alert alert-warning">
                    <a class="close" data-dismiss="alert">×</a>
                    <span class="label label-warning">Warning</span>

                    {% if message.error %} {% endif %}
                    {{ message }}
                </div>

            {% endfor %}
    {% endif %}
```
