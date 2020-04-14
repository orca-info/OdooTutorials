# 开发
## __*003*__ 模型(class)


### 一、熟悉odoo模型的 ORM API  
---
示例：创建模型
```python
from odoo import models, fields, api

class TestModel(modles.Modle):
    _name = 'test.model' //命名模型的名称格式：模块名.模型名
    _description = u'测试模块' //对模块的描述
    
    @api.depends('test_name')
    def create_test_id(self):
        print(dir(self))
        for record in self:
            if record.test_name:
                record.test_id = record.id

    test_name = fields.Char(string='Test', required=True) //定义Test名称字段，为必须
    test_id = fields.Interger(string='Test record id') //定义id字段
```
> odoo的模型都是从class odoo.models.Model(pool, cr)继承而来
模型的属性结构：
1._name 业务对象的名称  
2._rec_name 可选的name字段名称，供osv的name_get()方法使用，默认值name  
3._inherit 如果设置了__name属性，它的取值是单个或多个父级的模型名称；没有设置__name属性时，只能是单个模型名称  
4._order 在搜索的时候的默认排序，默认值是id  
5._auto 指定该表是否需要创建，默认值是True，如果设置成False需要重写init方法来创建表  
6._table 当_auto设置成false时，该值为创建的表名；默认情况下会自动生成一个  
7._inherits 定义上级模型关联使用的外键  
[简书](https://www.jianshu.com/p/88f6f79756be)  
[Odoo ORM API](https://www.odoo.com/documentation/12.0/reference/orm.html)
## <font color=blue>__slef:__ </font>  
self对象-是指当前模型对象自己，并把整个模型的API封装在self，包括方法和对象，如：cr（游标）、id（用户ID)、context（上下文）、records（记录集）、session（缓存）、env（环境）等等，是一个数据集合。
>_使用方法：self.xx 获取对象、self.xx() 获取方法_  
_如：self.cr() （获取游标方法）、self.records（获取记录集）_   

打印self对象的属性和方法：
```shell
self: ['<lambda>', 'CONCURRENCY_CHECK_FIELD', '_Attachment', '_BaseModel__ensure_xml_id', '_BaseModel__export_rows', '__add__', '__and__', '__bool__', '__class__', '__contains__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__int__', '__iter__', '__last_update', '__le__', '__len__', '__lt__', '__module__', '__ne__', '__new__', '__nonzero__', '__or__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__weakref__', '_abstract', '_add_fake_fields', '_add_field', '_add_inherited_fields', '_add_magic_fields', '_add_missing_default_values', '_add_sql_constraints', '_apply_ir_rules', '_auto', '_auto_init', '_browse', '_build_model', '_build_model_attributes', '_build_model_check_base', '_build_model_check_parent', '_build_search_childs_domain', '_cache', '_check_concurrency', '_check_context_bin_size', '_check_m2m_recursion', '_check_parent_field', '_check_qorder', '_check_recursion', '_check_removed_columns', '_compute_activity_date_deadline', '_compute_activity_state', '_compute_display_name', '_compute_is_follower', '_compute_message_attachment_count', '_compute_message_has_error', '_compute_upper_name', '_constraint_methods', '_constraints', '_context', '_convert_records', '_convert_to_cache', '_convert_to_record', '_convert_to_write', '_cr', '_create', '_create_parent_columns', '_custom', '_date_name', '_depends', '_description', '_execute_sql', '_export_rows', '_extract_records', '_field_computed', '_field_inverses', '_field_triggers', '_fields', '_fields_view_get', '_filter_access', '_filter_access_ids', '_filter_access_rules', '_find_partner_from_emails', '_fold_name', '_garbage_collect_attachments', '_generate_m2o_order_by', '_generate_order_by', '_generate_order_by_inner', '_generate_translated_field', '_get_default_activity_view', '_get_default_calendar_view', '_get_default_form_view', '_get_default_graph_view', '_get_default_kanban_view', '_get_default_pivot_view', '_get_default_search_view', '_get_default_sms_recipients', '_get_default_tree_view', '_get_dirty', '_get_external_ids', '_get_followers', '_get_message_needaction', '_get_message_unread', '_get_tracked_fields', '_get_xml_ids', '_has_onchange', '_ids', '_in_cache_without', '_inherit', '_inherit_children', '_inherits', '_inherits_check', '_inherits_children', '_inherits_join_add', '_inherits_join_calc', '_init_column', '_init_constraints_onchanges', '_inverse_upper_name', '_is_an_ordinary_table', '_is_dirty', '_load_records', '_load_records_create', '_load_records_write', '_local_constraints', '_local_sql_constraints', '_log_access', '_mail_flat_thread', '_mail_post_access', '_mail_post_token_field', '_mapped_cache', '_mapped_func', '_message_add_suggested_recipient', '_message_auto_subscribe', '_message_auto_subscribe_followers', '_message_auto_subscribe_notify', '_message_extract_payload', '_message_extract_payload_postprocess', '_message_find_partners', '_message_log', '_message_post_after_hook', '_message_post_process_attachments', '_message_subscribe', '_message_track', '_message_track_get_changes', '_message_track_post_template', '_model_cache_key', '_module', '_name', '_name_search', '_needaction', '_notify_classify_recipients', '_notify_classify_recipients_on_records', '_notify_email_recipients', '_notify_email_recipients_on_records', '_notify_encode_link', '_notify_get_action_link', '_notify_get_groups', '_notify_get_reply_to', '_notify_get_reply_to_on_records', '_notify_specific_email_values', '_notify_specific_email_values_on_records', '_onchange_eval', '_onchange_methods', '_onchange_spec', '_order', '_original_module', '_parent_name', '_parent_store', '_parent_store_compute', '_parent_store_create', '_parent_store_update', '_parent_store_update_prepare', '_patch_method', '_pop_field', '_prefetch', '_prefetch_field', '_prepare_setup', '_proper_fields', '_read_from_database', '_read_group_fill_results', '_read_group_fill_temporal', '_read_group_format_result', '_read_group_prepare', '_read_group_prepare_data', '_read_group_process_groupby', '_read_group_raw', '_read_group_resolve_many2one_fields', '_rec_name', '_rec_name_fallback', '_recompute_check', '_recompute_done', '_recompute_todo', '_reflect', '_register', '_register_hook', '_replace_local_links', '_revert_method', '_routing_create_bounce_email', '_routing_warn', '_search', '_search_activity_date_deadline', '_search_activity_summary', '_search_activity_type_id', '_search_activity_user_id', '_search_follower_channels', '_search_follower_partners', '_search_is_follower', '_search_message_has_error', '_search_message_needaction', '_search_on_partner', '_search_on_user', '_search_parents', '_sequence', '_set_dirty', '_setup_base', '_setup_complete', '_setup_done', '_setup_fields', '_sql_constraints', '_table', '_table_has_rows', '_track_subtype', '_track_template', '_transient', '_transient_check_count', '_transient_clean_old_rows', '_transient_clean_rows_older_than', '_transient_max_count', '_transient_max_hours', '_transient_vacuum', '_translate', '_uid', '_validate_fields', '_where_calc', '_write', 'action_patients', 'action_send_patient_card', 'active', 'activity_date_deadline', 'activity_feedback', 'activity_ids', 'activity_reschedule', 'activity_schedule', 'activity_schedule_with_view', 'activity_send_mail', 'activity_state', 'activity_summary', 'activity_type_id', 'activity_unlink', 'activity_user_id', 'age_group', 'appointment_count', 'browse', 'check_access', 'check_access_rights', 'check_access_rule', 'check_age', 'check_field_access_rights', 'check_mail_message_access', 'clear_caches', 'compute_concurrency_field', 'compute_concurrency_field_with_access', 'concat', 'copy', 'copy_data', 'copy_translations', 'create', 'create_date', 'create_uid', 'default_get', 'display_name', 'doctor_gender', 'email_id', 'ensure_one', 'env', 'exists', 'export_data', 'fields_get', 'fields_get_keys', 'fields_view_get', 'filtered', 'gender', 'get_access_action', 'get_appointments', 'get_empty_list_help', 'get_external_id', 'get_formview_action', 'get_formview_id', 'get_import_templates', 'get_metadata', 'get_xml_id', 'id', 'ids', 'image', 'init', 'invalidate_cache', 'is_transient', 'load', 'load_views', 'mapped', 'message_attachment_count', 'message_change_thread', 'message_channel_ids', 'message_follower_ids', 'message_get_default_recipients', 'message_get_suggested_recipients', 'message_has_error', 'message_has_error_counter', 'message_ids', 'message_is_follower', 'message_main_attachment_id', 'message_needaction', 'message_needaction_counter', 'message_new', 'message_notify', 'message_parse', 'message_partner_ids', 'message_partner_info_from_emails', 'message_post', 'message_post_send_sms', 'message_post_with_template', 'message_post_with_view', 'message_process', 'message_receive_bounce', 'message_route', 'message_route_process', 'message_route_verify', 'message_subscribe', 'message_track', 'message_unread', 'message_unread_counter', 'message_unsubscribe', 'message_update', 'modified', 'name', 'name_create', 'name_get', 'name_search', 'name_seq', 'new', 'notes', 'onchange', 'open_patient_appointment', 'patient_age', 'patient_age2', 'patient_doctor', 'patient_name', 'patient_name_upper', 'pool', 'print_report', 'print_report_excel', 'read', 'read_group', 'read_progress_bar', 'recompute', 'refresh', 'resolve_2many_commands', 'resolve_o2m_commands_to_record_dicts', 'search', 'search_childs', 'search_count', 'search_panel_select_multi_range', 'search_panel_select_range', 'search_parents', 'search_read', 'search_read_childs', 'search_read_parents', 'set_age_group', 'set_doctor_gender', 'sorted', 'states', 'sudo', 'test_cron_job', 'toggle_active', 'union', 'unlink', 'update', 'user_has_groups', 'user_id', 'view_header_get', 'view_init', 'website_message_ids', 'with_context', 'with_env', 'with_prefetch', 'write', 'write_date', 'write_uid']
```

__self常用API：__  
> __*env*__: __env__(运行环境)中存储了模型的缓存记录集和ORM相关的变量：cr（数据库查询游标）、user（当前用户）、元数据，通过self.env来访问，因此可对模型记录操作 __增、删、查、改__ ,从而达到修改数据库的记录。例如：  
 ```python
 // create(dics)：创建新纪录
 // unlink()：删除记录
 // search_read()：查询记录,返回符合条件的dics列表
 // write(dics)：修改记录
 // browse([ids]):浏览对象（一个或多个）,其中browse(id).read(fields)f返回记录集的指定字段（dics）
 // search:搜索search([domain])，其中search([domain]).read(fields)返回记录集的指定字段（disc）

 // vals是一个dics{'key':'value'}
 self.env['模型'].create(vlas) 

 //改变用户权限：通过sudo()获得超级管理员权限，确保操作能够进行
 self.env['模型'].sudo().create(vals)
 ```
__self.env对象的属性和方法：__
 ```shell
 self.env attrs: ['__abstractmethods__', '__call__', '__class__', '__contains__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', '__weakref__', '_abc_cache', '_abc_negative_cache', '_abc_negative_cache_version', '_abc_registry', '_cache_key', '_do_in_mode', '_local', '_protected', 'add_todo', 'all', 'args', 'cache', 'cache_key', 'check_todo', 'clear', 'clear_upon_failure', 'context', 'cr', 'dirty', 'do_in_draft', 'do_in_onchange', 'envs', 'field_todo', 'get', 'get_todo', 'has_todo', 'in_draft', 'in_onchange', 'items', 'keys', 'lang', 'manage', 'norecompute', 'protected', 'protecting', 'recompute', 'ref', 'registry', 'remove_todo', 'reset', 'uid', 'user', 'values']
 ```
 ```shell
 // 访问当前用户
 self.env.user

// 获取XML的ID
self.env.ref('external_id')
 ```
 > 详情请参考官方ORM文档[Odoo ORM API](https://www.odoo.com/documentation/12.0/reference/orm.html)
