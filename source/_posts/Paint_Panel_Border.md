---
title: 重绘Panel边框线
toc: true
top: 100
mathjax: false
date: 2019-01-21 22:00:53
tags:
- winform
- panel重绘
categories:
- C#
- winform
---
# 序
最近有一个需求,是重绘panel边框的颜色成指定的颜色,参考了网上的一些示例代码和结合实际情况,形成了自己的解决方案,以下是实现过程。

# 示例代码

## 边框绘制类
```
    /// <summary>
    /// panel边框绘制类
    /// </summary>
    public static class PanelBorderPainter
    {
        private static Color _colorPanelBorder = Color.FromArgb(215, 215, 215); //d7d7d7
        private static int BorderSize = 1;

        /// <summary>
        /// 重绘panel的边框颜色
        /// <para>Padding 需要设置为1,例如:panel.Padding = new System.Windows.Forms.Padding(1);</para>
        /// <para>默认颜色值是d7d7d7</para>
        /// <para>可以通过panel.Tag=Color.FromArgb(255, 0, 0)设置颜色为红色</para>
        ///<para>注意:该Paint事件应置于InitializeComponent()之后实现,重新生成Designer.cs会导致代码丢失。</para>
        /// 
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        public static void Paint(object sender, PaintEventArgs e)
        {
            if (sender is Control control)
            {
                #region panel颜色信息通过Tag传递

                if (control.Tag != null)
                {
                    if (control.Tag is Color color)
                    {
                        _colorPanelBorder = color;
                    }
                }

                #endregion


                ButtonBorderStyle style = ButtonBorderStyle.Solid;
                ControlPaint.DrawBorder(e.Graphics, control.ClientRectangle,
                    _colorPanelBorder, BorderSize, style,
                    _colorPanelBorder, BorderSize, style,
                    _colorPanelBorder, BorderSize, style,
                    _colorPanelBorder, BorderSize, style);
            }
        }
    }
```

## 声明两个Panel

<font color=#FF0000>容器panelOuter包含panelInner</font>
```
private System.Windows.Forms.Panel panelOuter;
private System.Windows.Forms.Panel panelInner;
```

## 关键代码

panelOuter
```
this.panelOuter.Padding = new System.Windows.Forms.Padding(1); //Padding属性设置为1。
this.panelOuter.Anchor = ((System.Windows.Forms.AnchorStyles)((((System.Windows.Forms.AnchorStyles.Top | System.Windows.Forms.AnchorStyles.Bottom) 
            | System.Windows.Forms.AnchorStyles.Left) 
            | System.Windows.Forms.AnchorStyles.Right)));
 this.panelOuter.Controls.Add(panelInner); //必须
```

panelInner
```
this.panelInner.Dock = System.Windows.Forms.DockStyle.Fill;
```
Form1构造函数
```
        public Form1()
        {
            InitializeComponent();
            this.panelOuter.Paint += new PaintEventHandler(PanelBorderPainter.Paint);
        }

```

## 答疑

<font color=#FF0000>为什么需要两个Panel?</font>  
答:因为如果注释掉以下代码,水平拖拽时会出现如下的界面效果。
```
this.panelOuter.Controls.Add(panelInner);
```
![错误重绘示例](错误重绘示例.png)
取消该行代码注释则页面拖拽效果正常。

<font color=#FF0000>是否必须需要两个Panel?</font>  
答:不是,内部是**DataGridView**等控件,其Dock=DockStyle.Fill,可以起到同样的效果。

# 参考资料
* [Winform的Panel绘制边框](https://blog.csdn.net/softimite_zifeng/article/details/54237134)