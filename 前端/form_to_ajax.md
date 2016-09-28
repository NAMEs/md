#from转化为ajax提交

##优点
不用跳转，用户体验好，不用担心各种返回的问题，方便一些比如layer的前端插件使用，美化界面；
##一个例子  
表单部分：  

    <div class="modal fade" id="myDialog" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <?php
            $form = ActiveForm::begin([
                        'id' => 'edit-form',
                        'action' => Url::toRoute('record/edit'),
                        'options' => ['class' => 'form-horizontal'],
                        'fieldConfig' => [
                            'options' => ['class' => NULL],
                        ],
            ]);
            ?>
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                    <h4 class="modal-title">Modal title</h4>
                </div>
                <div class="modal-body">
                    <?= $form->field($model, 'id', ['template' => '{input}', 'options' => ['stype' => 'display:none']])->hiddenInput()->label(false) ?>
                    <div class="form-group">
                        <?= $form->field($model, 'channel_id', ['template' => '{label}<div class="col-md-2">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']])->dropDownList($listChannel) ?>
                        <?= $form->field($model, 'channel_note', ['template' => '{label}<div class="col-md-4">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']]) ?>
                        <div class="col-md-2">
                            <?= $form->field($model, 'is_valid')->checkbox()->label('有效对话') ?>
                        </div>
                        <div class="col-md-2">
                            <?= $form->field($model, 'is_reserve')->checkbox()->label('有效预约') ?>
                        </div>
                    </div>
                    <div class="form-group">
                        <?= $form->field($model, 'department_id', ['template' => '{label}<div class="col-md-2">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']])->dropDownList($listDepartment) ?>
                        <?= $form->field($model, 'name', ['template' => '{label}<div class="col-md-3">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']]) ?>
                        <?= $form->field($model, 'phone', ['template' => '{label}<div class="col-md-4">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']]) ?>
                    </div>
                    <div class="form-group">
                        <?= $form->field($model, 'doctor_id', ['template' => '{label}<div class="col-md-2">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']])->dropDownList($listDoctor, ['prompt' => '不指定']) ?>
                        <?= $form->field($model, 'appointment', ['template' => '{label}<div class="col-md-4">{input}{hint}{error}</div>', 'inputOptions' => ['readonly' => true], 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']]) ?>
                        <?= Html::button('清除时间', ['class' => 'btn btn-default', 'onclick' => '$("#record-appointment").val("")']) ?>
                    </div>
                    <div class="form-group">
                        <?= $form->field($model, 'note', ['template' => '{label}<div class="col-md-10">{input}{hint}{error}</div>', 'labelOptions' => ['class' => 'control-label col-md-1', 'style' => 'padding-left:5px;padding-right:5px']])->textarea() ?>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <?= Html::submitButton('保存', ['class' => 'btn btn-primary']) ?>
                </div>
            </div>
            <?php ActiveForm::end(); ?>
        </div>
    </div>

表单绑定的提交前事件：  

	$("#edit-form").on('beforeSubmit',function(e){//注册了一个事件对应的函数，这个事件是本身自带的，是在提交表单前就会触发的
	ajaxSubmitForm($(this),function(data){
	   if(data.status==1){
	   	getList();
	    $('#myDialog').modal('hide');
	    }
	   });
	   return false;//由于这里返回false，所以表单不会进行提交；而是会跑到ajaxSubmitForm进行ajax提交，相应的layer弹框也是在那里完成，这里能实现代码冗余行大大减少，不用每个表单提交都进行处理
	});  
这边是被调用的ajax提交，代码适用性比较强  

	//ajax提交表单。
	function ajaxSubmitForm(form, func)
	{
	    $.ajax({
	        url: $(form).attr('action'),
	        type: $(form).attr('method'),
	        data: $(form).serialize(),//输出序列化的表单值
	        beforeSend: function () {
	            layer.load();
	        },
	        complete: function () {
	            layer.closeAll('loading');
	        },
	        error: function (XMLHttpRequest, textStatus, errorThrown) {
	            layer.alert('出错拉:' + textStatus + ' ' + errorThrown, {icon: 5});
	        },
	        success: function (data) {
	            if (!$.isFunction(func)) {
	                func = function (data) {
	                    data.url ? window.location.href = data.url : null;
	                }
	            }
	            if (data.status == 1)
	            {
	                layer.alert(data.message, {icon: 6}, function (index) {//修改之后提示信息弹框
	                    layer.close(index);
	                    func(data);
	                });
	            }
	            else {
	                layer.alert(data.message, {icon: 5}, function (index) {
	                    layer.close(index);
	                    func(data);
	                });
	            }
	        }
	    });
	    return false;
	}


其中，$(form).serialize() 是获取表单的值，输出为



