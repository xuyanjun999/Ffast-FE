<template>
  <div class="main-view main-view-full" style="padding-top: 50px">
    <row>
      <i-col span="24">
        <FormDynamic ref="dynamic1" v-model="fromData" :data="dynamic1" :label-width="120">
        </FormDynamic>
      </i-col>
    </row>
    <PopupEdit ref="popupEdit"
               :width="1000"
               :dynamic="dynamic1"
               @on-success="editSuccess"
               :label-width="120">
    </PopupEdit>
    <PopupSelect ref="popSelect" :content="userPage"></PopupSelect>
  </div>
</template>
<script>
  import {FormDynamic, PopupEdit, PopupSelect} from 'components/';

  let self = null;
  const dynamic1 = [
      [
        {type: 'title', span: 24, text: 'Input'}
      ],
      [
        {
          name: 'numberData',
          type: 'number',
          span: 6,
          label: 'Number',
          min: 1,
          max: 88888
        },
        {
          name: 'textData',
          type: 'text',
          span: 6,
          label: 'text',
          value: 'DefaultValue',
          rules: {required: true, type: 'string'}
        },
        {
          name: 'passwordData',
          type: 'text',
          span: 6,
          password: true,
          label: 'PasswordText',
          rules: {required: true, type: 'string', message: '密码不能为空'}
        },
        {
          name: 'textarea',
          type: 'text',
          placeholder: '多行文本输入框',
          span: 6,
          label: 'TextareaLabel',
          textarea: {minRows: 1, maxRows: 6}
        }
      ], [
        {type: 'title', span: 24, text: 'date'}
      ], [
        {name: 'date1', type: 'date', span: 6, label: 'DateLabel'},
        {name: 'date2', type: 'datetime', span: 6, label: 'DateTimeLabel'},
        {name: 'date3', type: 'datetimerange', span: 6, label: 'Datetimerange'}
      ],
      [
        {type: 'title', span: 24, text: 'DataSelect'}
      ],
      [
        {
          name: 'selectData',
          type: 'select',
          span: 8,
          value: 0,
          label: 'SelectLabel',
          data: [
            {label: '选项1', value: 0}, {label: '选项2', value: 1},
            {label: '选项3', value: 2}, {label: '选项4', value: 3}
          ],
          onChange: (val, from, data) => {
            alert('onChange');
          }
        },
        {
          name: 'selectData2',
          type: 'select',
          span: 6,
          value: 0,
          label: 'URLSelectLabel',
          // value 字段名
          valField: 'id',
          // label 字段名
          textField: 'name',
          // 通过设置数据url地址进行获取
          dataUrl: '/sys/dict/get?type=job',
          onChange: (val, from, data) => {
            alert('onChange');
          }
        },
        {
          name: 'selectData3',
          type: 'select',
          span: 6,
          value: 0,
          label: 'DictSelectLabel',
          // 直接取字典数据
          dict: 'job',
          onChange: (val, from, data) => {
            alert('onChange');
          }
        }
      ], [
        {type: 'title', span: 24, text: 'Title'}
      ], [
        {
          name: 'radioData',
          type: 'radio',
          span: 6,
          value: 0,
          label: 'RadioLabel',
          data: [
            {label: '选项1', value: 0}, {label: '选项2', value: 1},
            {label: '选项3', value: 2}, {label: '选项4', value: 3}
          ],
          onChange: (val, from, data) => {
            alert('onChange');
          }
        },
        {
          name: 'checkboxData',
          type: 'checkbox',
          span: 8,
          label: 'CheckboxLabel',
          data: [
            {label: '选项1', value: 0}, {label: '选项2', value: 1},
            {label: '选项3', value: 2}, {label: '选项4', value: 3}
          ],
          enableCheckAll: true,
          checkAllLabel: '全选',
          checkAll: true,
          onChange: (val, from, data) => {
            alert('onChange');
          }
        },
        {
          name: 'deviceTypeId',
          type: 'treeSelect',
          span: 8,
          label: 'TreeSelectLabel',
          // 如果是CrudView 支持从左边树绑定数据，select组件同样支持
          dataFromTree: true,
          textField: 'name',
          valField: 'id',
          dataUrl: '/sys/res/list',
          onChange: (val, from, data) => {
            alert('onChange');
          }
        },
        {
          name: 'popSelectId',
          span:
            8,
          label:
            'PopupSelectLabel',
          type:
            'popText',
          textField:
            'popSelectName',
          onClick(fromData) {
            self.$refs['popSelect'].open((selection) => {
              console.log(selection[0].id);
              self.$set(fromData, 'popSelectId', selection[0].id);
              self.$set(fromData, 'popSelectName', selection[0].username);
            })
          }
        }
      ],
      [
        {
          name: 'imgData',
          // 最多只能上传2张
          max: 2,
          type: 'imgUpload',
          span: 24,
          label: 'ImgUploadLabel'
        },
        {name: 'editor', type: 'editor', span: 24, label: 'EditorLabel', placeholder: '富文本编辑器'}
      ],
      [
        {
          name: 'button', type: 'buttons',
          span: 24,
          data: [
            {
              label: 'SetData',
              onClick() {
                // 给表单设置数据
                self.$refs.dynamic1.setFormData({numberData: 10001, textData: 'SetData', editor: '<h1>Fffast-FE</h1>'});
              }
            },
            {
              label: 'GetFormData',
              onClick() {
                self.$refs.dynamic1.submit((param) => {
                  console.log(param);
                  alert(JSON.stringify(param))
                }, (res) => {
                  // 表单验证失败
                });
              }
            },
            {
              // 弹出窗口编辑表单
              label: 'PopupWindow',
              onClick() {
                self.$refs.popupEdit.open({
                  title: 'PopupEditWindow',
                  // 确认提交url
                  postUrl: null
                }, self.fromData);
              }
            },
            {
              label: 'GotoEditPage',
              onClick() {
                self.editOptions.editSuccess = self.editSuccess;
                let action = {
                  // 窗口标题
                  title: 'title',
                  // 确认提交请求url
                  postUrl: ''
                }
                self.$router.push({
                  path: self.$router.currentRoute.path + '/edit',
                  query: {options: self.editOptions, action: action, data: self.fromData}
                });
              }
            }
          ]
        }
      ]
    ]
  ;

  /**
   * 弹出式表单参数
   */
  const editOptions = {
    width: 1200,
    labelWidth: 120,
    dynamic: dynamic1
  };

  export default {
    data() {
      return {
        editOptions,
        dynamic1,
        fromData: {}
      }
    },
    computed: {
      // 弹出选择页面
      userPage() {
        return import('pages/sys/user');
      }
    },
    methods: {
      // 编辑成功
      editSuccess(res) {
        console.log(res);
      }
    },
    mounted() {
      self = this;
    },
    components: {
      FormDynamic, PopupEdit, PopupSelect
    }
  }
</script>
