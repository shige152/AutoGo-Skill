---
name: "uiacc - 节点操作"
description: "提供基于无障碍服务的UI节点操作API，包括节点查找、点击、长按、滑动、文本输入、节点信息获取、控件遍历等。无需开启APP无障碍服务。当用户需要定位、交互或操作UI控件时调用。"
---

# Uiacc 模块 API 文档

提供基于辅助功能服务的控件定位、交互操作等功能。无需开启 APP 的无障碍服务。

## 函数速查

### Uiacc 选择器方法

| 分类 | 方法 | 简述 |
|------|------|------|
| 文本匹配 | Text / TextContains / TextStartsWith / TextEndsWith / TextMatches | text精确/包含/前缀/后缀/正则匹配 |
| 描述匹配 | Desc / DescContains / DescStartsWith / DescEndsWith / DescMatches | desc精确/包含/前缀/后缀/正则匹配 |
| ID匹配 | Id / IdContains / IdStartsWith / IdEndsWith / IdMatches | id精确/包含/前缀/后缀/正则匹配 |
| 类名匹配 | ClassName / ClassNameContains / ClassNameStartsWith / ClassNameEndsWith / ClassNameMatches | 类名精确/包含/前缀/后缀/正则匹配 |
| 包名匹配 | PackageName / PackageNameContains / PackageNameStartsWith / PackageNameEndsWith / PackageNameMatches | 包名精确/包含/前缀/后缀/正则匹配 |
| 位置 | Bounds / BoundsInside / BoundsContains | 精确范围/范围内/包含范围 |
| 布尔属性 | Clickable / LongClickable / Checkable / Selected / Enabled | 可点击/可长按/可选中/已选中/已启用 |
| 布尔属性 | Scrollable / Editable / MultiLine / Checked / Focusable | 可滚动/可编辑/多行/已勾选/可聚焦 |
| 布尔属性 | Dismissable / Focused / ContextClickable / Visible / Password | 可解散/已聚焦/上下文点击/可见/密码 |
| 其他 | DrawingOrder / Index | 绘制顺序/索引 |

### Uiacc 查找与操作

| 方法 | 简述 |
|------|------|
| New | 创建Accessibility对象 |
| Click | 点击指定文本 |
| WaitFor | 等待控件出现 |
| FindOnce | 查找单个控件 |
| Find | 查找所有符合条件的控件 |
| Release | 释放资源 |

### UiObject 操作方法

| 分类 | 方法 | 简述 |
|------|------|------|
| 点击 | Click / ClickCenter / ClickLongClick | 点击/中心点击/长按 |
| 编辑 | Copy / Cut / Paste / SetText | 复制/剪切/粘贴/设置文本 |
| 滚动 | ScrollForward / ScrollBackward | 向前/向后滚动 |
| 折叠 | Collapse / Expand | 折叠/展开 |
| 选中 | Select / ClearSelect / SetSelection | 选中/清除选中/设置选中范围 |
| 其他 | Show / SetVisibleToUser | 显示/设置可见性 |

### UiObject 属性获取

| 方法 | 简述 |
|------|------|
| GetText / GetDesc / GetId | 获取文本/描述/ID |
| GetClassName / GetPackageName | 获取类名/包名 |
| GetBounds / GetBoundsInParent | 获取屏幕范围/父控件范围 |
| GetParent / GetChild / GetChildren | 获取父控件/子控件/所有子控件 |
| GetChildCount / GetIndex / GetDrawingOrder | 获取子控件数/索引/绘制顺序 |
| ToString | 转文本 |

## Rect 结构体

以下是 uiacc 包中定义的 Rect 结构体及其字段说明：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| Left | int | 矩形的左边界 |
| Right | int | 矩形的右边界 |
| Top | int | 矩形的上边界 |
| Bottom | int | 矩形的下边界 |
| CenterX | int | 矩形的中心 X 坐标 |
| CenterY | int | 矩形的中心 Y 坐标 |
| Width | int | 矩形的宽度 |
| Height | int | 矩形的高度 |

## API 函数

### New

创建一个 Accessibility 对象。返回实例对象 `*Uiacc`

**参数：**
- displayId {int} 屏幕 ID，0 表示主屏幕，其他值表示虚拟屏幕，操作虚拟屏幕节点需要安卓版本大于等于 11

```go
acc := uiacc.New(0)
```

## 选择器属性设置方法

以下方法均返回 `*Uiacc` 对象，支持链式调用。

### Text

设置选择器的 text 属性。

**参数：**
- value {string} 文本值

```go
acc.Text("example text")
```

### TextContains

设置选择器的 textContains 属性。

**参数：**
- value {string} 包含的文本值

```go
acc.TextContains("example")
```

### TextStartsWith

设置选择器的 textStartsWith 属性。

**参数：**
- value {string} 以此文本开头

```go
acc.TextStartsWith("example")
```

### TextEndsWith

设置选择器的 textEndsWith 属性。

**参数：**
- value {string} 以此文本结尾

```go
acc.TextEndsWith("example")
```

### TextMatches

设置选择器的 textMatches 属性，用于匹配符合指定正则表达式的控件。

**参数：**
- value {string} 正则表达式

```go
acc.TextMatches("^example.*")
```

### Desc

设置选择器的 desc 属性，用于匹配描述等于指定文本的控件。

**参数：**
- value {string} 描述的文本值

```go
acc.Desc("example description")
```

### DescContains

设置选择器的 descContains 属性，用于匹配描述包含指定文本的控件。

**参数：**
- value {string} 包含的描述文本值

```go
acc.DescContains("example")
```

### DescStartsWith

设置选择器的 descStartsWith 属性，用于匹配描述以指定文本开头的控件。

**参数：**
- value {string} 描述文本的开头

```go
acc.DescStartsWith("example")
```

### DescEndsWith

设置选择器的 descEndsWith 属性，用于匹配描述以指定文本结尾的控件。

**参数：**
- value {string} 描述文本的结尾

```go
acc.DescEndsWith("example")
```

### DescMatches

设置选择器的 descMatches 属性，用于匹配描述符合指定正则表达式的控件。

**参数：**
- value {string} 正则表达式

```go
acc.DescMatches("^example.*")
```

### Id

设置选择器的 id 属性，用于匹配 ID 等于指定值的控件。

**参数：**
- value {string} ID 值

```go
acc.Id("example_id")
```

### IdContains

设置选择器的 idContains 属性，用于匹配 ID 包含指定值的控件。

**参数：**
- value {string} 包含的 ID 值

```go
acc.IdContains("example")
```

### IdStartsWith

设置选择器的 idStartsWith 属性，用于匹配 ID 以指定值开头的控件。

**参数：**
- value {string} ID 的开头值

```go
acc.IdStartsWith("example")
```

### IdEndsWith

设置选择器的 idEndsWith 属性，用于匹配 ID 以指定值结尾的控件。

**参数：**
- value {string} ID 的结尾值

```go
acc.IdEndsWith("example")
```

### IdMatches

设置选择器的 idMatches 属性，用于匹配 ID 符合指定正则表达式的控件。

**参数：**
- value {string} 正则表达式

```go
acc.IdMatches("^example.*")
```

### ClassName

设置选择器的 className 属性，用于匹配类名等于指定值的控件。

**参数：**
- value {string} 类名的值

```go
acc.ClassName("example_class")
```

### ClassNameContains

设置选择器的 classNameContains 属性，用于匹配类名包含指定值的控件。

**参数：**
- value {string} 包含的类名值

```go
acc.ClassNameContains("example")
```

### ClassNameStartsWith

设置选择器的 classNameStartsWith 属性，用于匹配类名以指定值开头的控件。

**参数：**
- value {string} 类名的开头值

```go
acc.ClassNameStartsWith("example")
```

### ClassNameEndsWith

设置选择器的 classNameEndsWith 属性，用于匹配类名以指定值结尾的控件。

**参数：**
- value {string} 类名的结尾值

```go
acc.ClassNameEndsWith("example")
```

### ClassNameMatches

设置选择器的 classNameMatches 属性，用于匹配类名符合指定正则表达式的控件。

**参数：**
- value {string} 正则表达式

```go
acc.ClassNameMatches("^example.*")
```

### PackageName

设置选择器的 packageName 属性，用于匹配包名等于指定值的控件。

**参数：**
- value {string} 包名的值

```go
acc.PackageName("com.example")
```

### PackageNameContains

设置选择器的 packageNameContains 属性，用于匹配包名包含指定值的控件。

**参数：**
- value {string} 包含的包名值

```go
acc.PackageNameContains("example")
```

### PackageNameStartsWith

设置选择器的 packageNameStartsWith 属性，用于匹配包名以指定值开头的控件。

**参数：**
- value {string} 包名的开头值

```go
acc.PackageNameStartsWith("com.example")
```

### PackageNameEndsWith

设置选择器的 packageNameEndsWith 属性，用于匹配包名以指定值结尾的控件。

**参数：**
- value {string} 包名的结尾值

```go
acc.PackageNameEndsWith("example")
```

### PackageNameMatches

设置选择器的 packageNameMatches 属性，用于匹配包名符合指定正则表达式的控件。

**参数：**
- value {string} 正则表达式

```go
acc.PackageNameMatches("^com\.example.*")
```

### Bounds

设置选择器的 bounds 属性，用于匹配控件在屏幕上的范围。

**参数：**
- left, top, right, bottom {int} 控件的屏幕边界

```go
acc.Bounds(0, 0, 100, 100)
```

### BoundsInside

设置选择器的 boundsInside 属性，用于匹配控件在屏幕内的范围。

**参数：**
- left, top, right, bottom {int} 屏幕内的范围

```go
acc.BoundsInside(0, 0, 500, 500)
```

### BoundsContains

设置选择器的 boundsContains 属性，用于匹配控件包含在指定范围内。

**参数：**
- left, top, right, bottom {int} 包含的范围

```go
acc.BoundsContains(50, 50, 300, 300)
```

### DrawingOrder

设置选择器的 drawingOrder 属性，用于匹配控件在父控件中的绘制顺序。

**参数：**
- value {int} 绘制顺序

```go
acc.DrawingOrder(2)
```

## 布尔属性设置方法

以下方法用于设置控件的布尔属性，均返回 `*Uiacc` 对象。

### Clickable

设置选择器的 clickable 属性，用于匹配控件是否可点击。

**参数：**
- value {bool} 是否可点击

```go
acc.Clickable(true)
```

### LongClickable

设置选择器的 longClickable 属性，用于匹配控件是否可长按。

**参数：**
- value {bool} 是否可长按

```go
acc.LongClickable(true)
```

### Checkable

设置选择器的 checkable 属性，用于匹配控件是否可选中。

**参数：**
- value {bool} 是否可选中

```go
acc.Checkable(false)
```

### Selected

设置选择器的 selected 属性，用于匹配控件是否被选中。

**参数：**
- value {bool} 是否被选中

```go
acc.Selected(true)
```

### Enabled

设置选择器的 enabled 属性，用于匹配控件是否启用。

**参数：**
- value {bool} 是否启用

```go
acc.Enabled(true)
```

### Scrollable

设置选择器的 scrollable 属性，用于匹配控件是否可滚动。

**参数：**
- value {bool} 是否可滚动

```go
acc.Scrollable(false)
```

### Editable

设置选择器的 editable 属性，用于匹配控件是否可编辑。

**参数：**
- value {bool} 是否可编辑

```go
acc.Editable(true)
```

### MultiLine

设置选择器的 multiLine 属性，用于匹配控件是否多行。

**参数：**
- value {bool} 是否多行

```go
acc.MultiLine(false)
```

### Checked

设置选择器的 checked 属性，用于匹配控件是否被勾选。

**参数：**
- value {bool} 是否勾选

```go
acc.Checked(true)
```

### Focusable

设置选择器的 focusable 属性，用于匹配控件是否可聚焦。

**参数：**
- value {bool} 是否可聚焦

```go
acc.Focusable(true)
```

### Dismissable

设置选择器的 dismissable 属性，用于匹配控件是否可解散。

**参数：**
- value {bool} 是否可解散

```go
acc.Dismissable(false)
```

### Focused

设置选择器的 focused 属性，用于匹配控件是否是辅助功能焦点。

**参数：**
- value {bool} 是否为辅助功能焦点

```go
acc.Focused(true)
```

### ContextClickable

设置选择器的 contextClickable 属性，用于匹配控件是否是上下文点击。

**参数：**
- value {bool} 是否为上下文点击

```go
acc.ContextClickable(false)
```

### Index

设置选择器的 index 属性，用于匹配控件在父控件中的索引。

**参数：**
- value {int} 索引值

```go
acc.Index(1)
```

### Visible

设置选择器的 visible 属性，用于匹配控件是否可见。

**参数：**
- value {bool} 是否可见

```go
acc.Visible(true)
```

### Password

设置选择器的 password 属性，用于匹配控件是否为密码字段。

**参数：**
- value {bool} 是否为密码字段

```go
acc.Password(false)
```

## 查找与操作方法

### Click

点击屏幕上的文本。

**参数：**
- text {string} 目标文本

```go
acc.Click("目标文本")
```

### WaitFor

等待控件出现，返回控件对象 `*UiObject`。

**参数：**
- timeout {int} 超时时间（毫秒）。0 表示无限等待

```go
obj := acc.Text("hello").WaitFor(3000)
```

### FindOnce

查找单个控件，成功返回控件对象 `*UiObject`。

```go
obj := acc.Text("hello").FindOnce()
```

### Find

查找所有符合条件的控件。返回控件对象数组 `[]*UiObject`。

```go
objects := acc.Text("hello").Find()
```

### Release

释放无障碍服务资源。

```go
uiacc.Release()
```

## UiObject 对象方法

以下方法用于操作查找到的控件对象。

### Click

点击该控件，并返回是否点击成功。

```go
success := uiObject.Click()
fmt.Println("点击成功:", success)
```

### ClickCenter

使用控件坐标点击该控件的中点。

```go
success := uiObject.ClickCenter()
fmt.Println("点击中心成功:", success)
```

### ClickLongClick

长按该控件，并返回是否点击成功。

```go
success := uiObject.ClickLongClick()
fmt.Println("长按成功:", success)
```

### Copy

对输入框文本的选中内容进行复制，并返回是否操作成功。

```go
success := uiObject.Copy()
fmt.Println("复制成功:", success)
```

### Cut

对输入框文本的选中内容进行剪切，并返回是否操作成功。

```go
success := uiObject.Cut()
fmt.Println("剪切成功:", success)
```

### Paste

对输入框控件进行粘贴操作，把剪贴板内容粘贴到输入框中，并返回是否操作成功。

```go
success := uiObject.Paste()
fmt.Println("粘贴成功:", success)
```

### ScrollForward

对控件执行向前滑动的操作，并返回是否操作成功。

```go
success := uiObject.ScrollForward()
fmt.Println("向前滑动成功:", success)
```

### ScrollBackward

对控件执行向后滑动的操作，并返回是否操作成功。

```go
success := uiObject.ScrollBackward()
fmt.Println("向后滑动成功:", success)
```

### Collapse

对控件执行折叠操作，并返回是否操作成功。

```go
success := uiObject.Collapse()
fmt.Println("折叠成功:", success)
```

### Expand

对控件执行展开操作，并返回是否操作成功。

```go
success := uiObject.Expand()
fmt.Println("展开成功:", success)
```

### Show

执行显示操作，并返回是否操作成功。

```go
success := uiObject.Show()
fmt.Println("显示成功:", success)
```

### Select

对控件执行"选中"操作，并返回是否操作成功。

```go
selected := uiObject.Select()
fmt.Println("控件是否选中成功:", selected)
```

### ClearSelect

清除控件的选中状态，并返回是否操作成功。

```go
cleared := uiObject.ClearSelect()
fmt.Println("控件是否清除选中成功:", cleared)
```

### SetSelection

对输入框控件设置选中的文字内容，并返回是否操作成功。

**参数：**
- start {int} 选中内容的起始位置
- end {int} 选中内容的结束位置

```go
success := uiObject.SetSelection(0, 5)
fmt.Println("设置选中内容是否成功:", success)
```

### SetVisibleToUser

设置控件是否可见。

**参数：**
- isVisible {bool} 是否可见

```go
success := uiObject.SetVisibleToUser(false)
fmt.Println("设置控件不可见是否成功:", success)
```

### SetText

设置输入框控件的文本内容，并返回是否设置成功。

**参数：**
- str {string} 文本内容

```go
success := uiObject.SetText("example text")
fmt.Println("设置文本是否成功:", success)
```

## UiObject 属性获取方法

以下方法用于获取控件的各种属性。

### GetClickable

获取控件的 clickable 属性。

```go
clickable := uiObject.GetClickable()
fmt.Println("控件是否可点击:", clickable)
```

### GetLongClickable

获取控件的 longClickable 属性。

```go
longClickable := uiObject.GetLongClickable()
fmt.Println("控件是否支持长按:", longClickable)
```

### GetCheckable

获取控件的 checkable 属性。

```go
checkable := uiObject.GetCheckable()
fmt.Println("控件是否可选中:", checkable)
```

### GetSelected

获取控件的 selected 属性。

```go
selected := uiObject.GetSelected()
fmt.Println("控件是否被选中:", selected)
```

### GetEnabled

获取控件的 enabled 属性。

```go
enabled := uiObject.GetEnabled()
fmt.Println("控件是否启用:", enabled)
```

### GetScrollable

获取控件的 scrollable 属性。

```go
scrollable := uiObject.GetScrollable()
fmt.Println("控件是否可滚动:", scrollable)
```

### GetEditable

获取控件的 editable 属性。

```go
editable := uiObject.GetEditable()
fmt.Println("控件是否可编辑:", editable)
```

### GetMultiLine

获取控件的 multiLine 属性。

```go
multiLine := uiObject.GetMultiLine()
fmt.Println("控件是否多行:", multiLine)
```

### GetChecked

获取控件的 checked 属性。

```go
checked := uiObject.GetChecked()
fmt.Println("控件是否被勾选:", checked)
```

### GetFocused

获取控件的 focused 属性。

```go
focused := uiObject.GetFocused()
fmt.Println("控件是否获得了输入焦点:", focused)
```

### GetFocusable

获取控件的 focusable 属性。

```go
focusable := uiObject.GetFocusable()
fmt.Println("控件是否可聚焦:", focusable)
```

### GetDismissable

获取控件的 dismissable 属性。

```go
dismissable := uiObject.GetDismissable()
fmt.Println("控件是否可解散:", dismissable)
```

### GetContextClickable

获取控件的 contextClickable 属性。

```go
contextClickable := uiObject.GetContextClickable()
fmt.Println("控件是否支持上下文点击:", contextClickable)
```

### GetVisible

获取控件的 visible 属性。

```go
visible := uiObject.GetVisible()
fmt.Println("控件是否可见:", visible)
```

### GetPassword

获取控件的 password 属性。

```go
password := uiObject.GetPassword()
fmt.Println("控件是否为密码字段:", password)
```

### GetAccessibilityFocused

获取控件的 AccessibilityFocused 属性。

```go
focused := uiObject.GetAccessibilityFocused()
fmt.Println("控件是否为辅助功能焦点:", focused)
```

### GetChildCount

获取控件的子控件数目。

```go
childCount := uiObject.GetChildCount()
fmt.Println("子控件数量:", childCount)
```

### GetDrawingOrder

获取控件在父控件中的绘制次序。

```go
drawingOrder := uiObject.GetDrawingOrder()
fmt.Println("控件绘制次序:", drawingOrder)
```

### GetIndex

获取控件在父控件中的索引。

```go
index := uiObject.GetIndex()
fmt.Println("控件在父控件中的索引:", index)
```

### GetBounds

获取控件在屏幕上的范围。

```go
bounds := uiObject.GetBounds()
fmt.Printf("控件范围：%v\n", bounds)
```

### GetBoundsInParent

获取控件在父控件中的范围。

```go
bounds := uiObject.GetBoundsInParent()
fmt.Println("控件在父控件中的范围:", bounds)
```

### GetId

获取控件的 ID。

```go
id := uiObject.GetId()
fmt.Println("控件 ID:", id)
```

### GetText

获取控件的文本内容。

```go
text := uiObject.GetText()
fmt.Println("控件文本内容:", text)
```

### GetDesc

获取控件的描述内容。

```go
desc := uiObject.GetDesc()
fmt.Println("控件描述内容:", desc)
```

### GetPackageName

获取控件的包名。

```go
packageName := uiObject.GetPackageName()
fmt.Println("控件包名:", packageName)
```

### GetClassName

获取控件的类名。

```go
className := uiObject.GetClassName()
fmt.Println("控件类名:", className)
```

### GetParent

获取控件的父控件。

```go
parent := uiObject.GetParent()
fmt.Println("控件的父控件:", parent)
```

### GetChild

获取控件的指定索引的子控件。

**参数：**
- index {int} 子控件的索引

```go
child := uiObject.GetChild(0)
fmt.Println("第一个子控件:", child)
```

### GetChildren

获取控件的所有子控件。返回控件对象数组 `[]*UiObject`。

```go
children := uiObject.GetChildren()
for index, child := range children {
    fmt.Printf("子控件 %d: %v\n", index+1, child)
}
```

### ToString

将节点对象转文本。

```go
str := uiObject.ToString()
fmt.Println("节点文本:", str)
```
