# PowerDesigner sql生成pdm，pdm的name和注释互换

# 一、File->New Model 点击OK创建模板就行了

![image](https://user-images.githubusercontent.com/64882640/125040961-ccdd4180-e0ca-11eb-85cd-c0800de5bf0f.png)

# 二、在PowerDesigner页面中点击选择适合你的SQL类型

![image](https://user-images.githubusercontent.com/64882640/125041039-e2eb0200-e0ca-11eb-9570-a5ad90bce400.png)

# 三、点击之后进入如下页面，有两个选择(1)using script files和(2)using a data  sourse
* (1)是选择sql语句

![image](https://user-images.githubusercontent.com/64882640/125041145-04e48480-e0cb-11eb-967e-e0e274867015.png)

* （2）是选择现有库，如下图

![image](https://user-images.githubusercontent.com/64882640/125041191-0f9f1980-e0cb-11eb-8104-6d5da118350a.png)

结束：到此为止就按照自己想要的表生成相应的PDM文件了。

# 四、 将pdm的name和注释互换位置

* 操作步骤： Open PDM – Tools – Execute Commands – Run Script

```
Option Explicit

ValidationMode = True
InteractiveMode = im_Batch

Dim mdl 'the current model

'get the current active model
Set mdl = ActiveModel
If (mdl Is Nothing) Then
MsgBox "There is no current Model"
ElseIf Not mdl.IsKindOf(PdPDM.cls_Model) Then
MsgBox "The current model is not an Physical Data model."
Else
ProcessFolder mdl
End If

'This routine copy name into code for each table, each column and each view
'of the current folder
Private sub ProcessFolder(folder)

Dim Tab 'running table
for each Tab in folder.tables
if not tab.isShortcut then
if len(tab.comment) <> 0 then
tab.name = tab.comment
end if
On Error Resume Next
Dim col 'running column
for each col in tab.columns
if len(col.comment) <>0 then
col.name =col.comment
end if
On Error Resume Next
next
end if
next
end sub

```

点击run即可
