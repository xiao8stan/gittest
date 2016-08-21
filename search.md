```
def get_task_list(max_results=0, keyword=''):
    task_list = []
    if keyword:
        task_list = Category.objects.filter(name__contains=keyword)

    if max_results > 0:
        if len(task_list) > max_results:
            task_list = task_list[:max_results]

    return task_list
    
    
    
  def suggestion_task(request):
      if request.method == 'GET':
          keyword = request.GET['keyword']
      else:
          pass
      task_list = get_task_list(10, keyword)
      return render(request, 'template.html', {'task_list': task_list}
```
