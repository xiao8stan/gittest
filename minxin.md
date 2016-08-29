>1. 当两段代码中都用到相同的部分代码,我们可以根据python面向对象集成机制来写个mixin类,虽然感觉很简单,但让我写的时候我还是没能写出来,
最后我的上司教我写了.


```
  class QuerysetMixin(object):
        def get_filter_queryset(self, field_map, key_value=None):
            queryset = Item.objects.filter(type='task')
    
            def construct_search(field_name):
                if field_name.startswith('^'):
                    return "%s__istartswith" % field_name[1:]
                elif field_name.startswith('='):
                    return "%s__iexact" % field_name[1:]
                elif field_name.startswith('@'):
                    return "%s__search" % field_name[1:]
                else:
                    return "%s__icontains" % field_name
    
            or_queries = []
            for key, val in field_map.items():
                orm_key = construct_search(str(key))
                orm_query = models.Q(**{orm_key: val})
                or_queries.append(orm_query)
    
            if or_queries:
                queryset = queryset.filter(reduce(operator.or_, or_queries))
    
            if key_value:
                queryset = queryset.values_list(key_value, flat=True)[:5]
            return queryset
    
    
    class SearchTaskDatatableView(LoginRequiredMixin, QuerysetMixin, DatatableView):
        model = Item
        column = [
            (_("Name"), 'summary', helpers.link_to_model),
            (_("Owner"), 'owner'),
            (_('Created'), 'create_time', 'date_format'),
            (_("Type"), 'type'),
            (_("Description"), 'description'), ]
        template_name = 'chart/search_results.html'
    
        def get_queryset(self):
            if self.request.GET.get('keyword') is not None:
                self.request.session['keyword'] = self.request.GET.get(
                    'keyword').strip()
            keyword = self.request.session.get('keyword', {})
            field_map = {'summary': keyword, 'description': keyword}
            queryset = self.get_filter_queryset(field_map)
            return queryset
    
        def get_context_data(self, **kwargs):
            context = super(SearchTaskDatatableView, self).get_context_data(
                **kwargs)
            context['goldcap_search'] = self.request.GET.get('keyword', {})
            return context
    
        def date_format(self, instance, *args, **kwargs):
            return instance.create_time.strftime("%Y-%m-%d")
    
        def get_datatable_options(self):
            return {
                'columns': self.column,
                'structure_template': 'datatableview/bootstrap_structure.html'
            }
    
    
    class AjaxsearchView(
        LoginRequiredMixin, QuerysetMixin, JSONResponseMixin,
        AjaxResponseMixin, View
    ):
        def get_ajax(self, request, *args, **kwargs):
            keyword = request.GET.get('term', {})
            field_map = {'summary': keyword, 'description': keyword}
            queryset_object = self.get_filter_queryset(field_map, 'summary')
            objects = list(queryset_object)
            return self.render_json_response(objects)


```
