##### **开发流程：**
###### 一、元数据设计
```
实现接口路径：
baseapp-->meta-->IBillInterface.bmf-->IOrgInfo、IAuditInfo、IbillNo、Irowno、
baseapp-->pf-->pfbiztf.bmf-->IflowBizItf、businInterface、IPFbillLock、
uapbs-->meta-->general_interface.bmf-->IBDObiect
导出为PDM文件，生成建库脚本
通过元数据生成VO类源码
```
###### 二、单据类型管理
```
注意事项：
1.需要在轻量端注册，并且在表体关联小应用，然后打开交易类型管理注册交易类型bd_billtype,插入一条全局脚本。
例如： 
49070202  联查  49070202_card     
49070202P 审批
2.单据动作脚本（NC端） (修改PUB_BILLACTION中的ActionType值，解决保存即提交的问题)
```
![68db9a38f21480b0f2baca34c41f9752.png](en-resource://database/1818:1)
###### 三、编码对象注册，编码规则定义
注意事项：
要导出预制的规则，如modules\ipmfund\config\billcodepredata\billcodepredata文件抽取编码规则脚本，在编码规则定义节点导出，导出后的目录在moudels下的对应模块中，复制到工程的目录下.
![daf1e3b11ad28c1f2c13f63a1877f986.png](en-resource://database/1820:0)
###### 四、应用注册，菜单注册（NCC轻量端）
```
注册模板时有个问题，每个模板的聚合类需要在后台插入
select CLAZZ from pub_page_templet order by ts desc
```
###### 五、卡片界面和列表界面的按钮注册，界面模板设置以及轻量端输出模板配置
```
可以通过NCC单据按钮注册脚本抽取，插入到新单据中
APPID 修改为对应节点的PARENT_ID（select PARENT_ID from sm_apppage where PAGECODE = '49020102_list'）
PAGECODE 修改为对应页面的PAGECODE
主键PK_BTN 前缀是APPID的前十八位 
例如：1001Y3100000000051BU 的1001Y3100000000051  后缀改成和已有数据不冲突的就行
需要修改的字段：APPID  PAGECODE ts  主键
```
###### 六、审批小应用注册，审批模板配置
```
工作任务消息配置节点，要设为默认（先注册消息模板全局）
```
###### 七、消息模板（消息模板类型注册）
```
消息标题：
【@&m billmaker.user_name@】提交的【固定资产投资完成情况报送】【@&m vbillcode@】待审批
消息内容：
<div class="divtext">
<span class="labeltext">项目组织:</span >
<span class="normaltext">@&m pk_org.name@</span >
</div>
<div class="divtext">
<span class="labeltext">单据名称:</span >
<span class="normaltext">@&m vbillname@</span >
</div>
<div class="divtext">
<span class="labeltext">经办部门:</span >
<span class="normaltext">@&m pk_dept.name@</span >
</div>
<div class="divtext">
<span class="labeltext">经办人:</span >
<span class="normaltext">@&m pk_psndoc.name@</span >
</div>
```
###### 八、数据权限资源设置
```
预制前要添加对应节点bpf文件，对应相关的业务操作
Approve 审批
Delete 删除
Edit 修改
UnApprove 弃审
```
###### 九、接口以及实现类
```
upm文件添加接口和实现类
```
###### 十、后台N_XXXX脚本实现类
```
select *from pub_billaction order by ts desc 动作脚本保存即提交问题
需要修改的的地方有 后端接口，VO，单据节点常量
```
###### 十一、NCC的映射文件
```
配置完执行下publish.bat
```
###### 十二、代码提交检查项
```
1、代码、元数据
2、pdm文件：组件-->script-->conf-->pdm
3、建库脚本：
  pdm文件-->右键pdm文件导出建库脚本-->选中目录：组件-->script-->dbcreate
4、预置脚本：
  items.xml --> 右键导出预置脚本
```
