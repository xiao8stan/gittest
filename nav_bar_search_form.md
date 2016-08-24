在网上搜了很多样式没有满意的，结合了许多人的样式，自己稍微调整了一下，可以用了

关键字搜索search input 谷歌有很多样式...
```
                <form class="navbar-form navbar-right" action="url" method="post" role="form">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="Search" name="keyword">
                    </div>
                    <button type="submit" class="btn"><span class="glyphicon glyphicon-search"></span></button>
                </form>
                
                
                                            <form class="navbar-form navbar-right" action="{% url 'search' %}" method="get"
                                  role="form" style="width: 20em;">
                                <div class="input-group">
                                    <input type="text" class="form-control" placeholder="Search" name="keyword">
                                    <div class="input-group-btn">
                                        <button type="submit" class="btn btn-info">
                                            <span class="glyphicon glyphicon-search"></span>
                                        </button>
                                    </div>
                                </div>
                            </form>
```
