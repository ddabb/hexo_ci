---
title: 重构技巧之添加参数
toc: true
top: 100
mathjax: false
date: 2019-03-10 09:32:02
tags:
- 重构
- TreeView
categories:
- 代码世界
- 重构
---
# 序
最近在经手一个Winform项目,里面有个需求：TreeView控件选择不同的TreeNode时，需要展示不同的右键菜单。记录一下重构过程。

## 需求
现将公司内部需求初始需求转换如下：
如果TreeView选中的TreeNode是国家，则右键菜单展示"添加省",一个选项。  
如果TreeView选中的TreeNode是省份，则右键菜单展示"添加市"，"删除"两个选项。  
如果TreeView选中的TreeNode是城市，则右键菜单展示"添加区"，"删除"两个选项。  
如果TreeView选中的TreeNode是区，则右键菜单展示，"删除"一个选项。
这部分的伪代码如下：

```
var area = TreeView.SelectedNode.Tag as Area; //
if(area!=null)
{
    var areaType=area.type;
    ContextMenuStrip menuStrip = new ContextMenuStrip();
    if(areaType==AreaType.国家)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("添加省"));
    }
    if(areaType==AreaType.省份)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("添加市"));
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));
    }
    if(areaType==AreaType.城市)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("添加区"));
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));
    }
    if(areaType==AreaType.区)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));
    }

    menuStrip.Show(e.Location);
}
```
后来业务发展了,除了AreaType.国家这个选项，右键菜单都需要添加一个"打开地图"的右键菜单，且位于右键菜单的最上方。
重构之后的示例代码变成了如下：
```
var area = TreeView.SelectedNode.Tag as Area; //
if(area!=null)
{
    var areaType=area.type;
    ContextMenuStrip menuStrip = new ContextMenuStrip();
    if(areaType==AreaType.国家)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("添加省"));
    }
    if(areaType==AreaType.省份)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("打开地图"));
        menuStrip.Items.Add(new ToolStripMenuItem("添加市"));
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));
    }
    if(areaType==AreaType.城市)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("打开地图"));
        menuStrip.Items.Add(new ToolStripMenuItem("添加区"));
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));

    }
    if(areaType==AreaType.区)
    {
        menuStrip.Items.Add(new ToolStripMenuItem("打开地图"));
        menuStrip.Items.Add(new ToolStripMenuItem("删除"));
    }

    menuStrip.Show(e.Location);
}
```
这次需求变动，已经让我闻到了代码中的坏味道。增删改一个右键菜单需要修改好几处地方，删除右键菜单也是同理。  
故需要对代码进行重构。

## 重构

提取函数，伪代码如下

```
private ContextMenuStrip menuStrip = new ContextMenuStrip(); //菜单转为私有属性
private void AddToolStripMenuItem(string controlName, bool isAdd)
{
    if (isAdd)
    {
        this.menuStrip.Items.Add(new ToolStripMenuItem(controlName););
    }
}
```
故调用处的实现代码变成了如下
```
var area = TreeView.SelectedNode.Tag as Area; //
if(area!=null)
{
    var areaType=area.type;
    this.menuStrip.Items.Clear();
    AddToolStripMenuItem("添加省",areaType==AreaType.国家);
    AddToolStripMenuItem("打开地图",!areaType==AreaType.国家);
    AddToolStripMenuItem("添加市",areaType==AreaType.省份);
    AddToolStripMenuItem("添加区",areaType==AreaType.城市);
    AddToolStripMenuItem("删除",!areaType==AreaType.国家);
    menuStrip.Show(e.Location);
}
```
若以后需求变更,**打开地图**需要改成**打开百度地图**，然后新增**打开高德地图**也只需要修改一行代码，并新增一行代码即可。

# 总结
该重构过程运用的是提取函数和函数添加参数。
**AddToolStripMenuItem函数**只关心菜单项是否添加，而不关心菜单项添加的逻辑是什么。  
重构之后，右键菜单的可能的菜单项也一目了然，每一个菜单项对应的添加条件也一目了然。若**isAdd**参数对应的表达式过长，则可以将该表达式封装成函数进行传递。  
通过该重构提高了项目的可维护性，也算是一种成就。
