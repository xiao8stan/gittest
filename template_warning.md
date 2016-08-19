```
{% if messages %}
        <ul class="messages">
            {% for message in messages %}
                <div class="alert alert-warning btn-warning">
                    <a class="close" data-dismiss="alert">×</a>
                    <span class="label label-warning">Warning</span>

                    {% if message.error %} {% endif %}
                    {{ message }}
                </div>

            {% endfor %}
        </ul>
    {% endif %}
```
