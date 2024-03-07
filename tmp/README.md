### 测试对象

![](./test-object.png)

### 测试目标

识别出对象中所有节点都存在中文

### 测试结果

### prompt

```
任务: 查找出图片中存在不合理的地方

## 超出容器
type OverflowAction = { action: "overflow", element: number, reason: string }
## 出现省略号
type EllipsisAction = { action: "ellipsis", element: number, reason: string }
## 不正常换行
type NewLineAction = { action: "newline", element: number, reason: string }
## 内容被遮挡
type BlockAction = { action: "block", element: number, reason: string }
## 内容被隐藏
type HiddenAction = { action: "hidden", element: number, reason: string }
## 内容存在中文
type ExitInChineseAction = { action: "exit-in-chinese", element: number, reason: string }

## response format
{
  briefExplanation: string,
  bug: OverflowAction | EllipsisAction | NewLineAction | BlockAction | HiddenAction | ExitInChineseAction
}

## response examples
{
  "briefExplanation": "I'll type 'funny cat videos' into the search bar",
  "bug": [
    {
        "action": "overflow",
        "element": 1,
        "reason": "超出容器范围"
    },
    {
        "action": "ellipsis",
        "element": 2,
        "reason": "文字出现省略号"
    }
  ]
}
{
  "briefExplanation": "Today's doodle looks interesting, I'll click it",
  "bug": [
    {
        "action": "hidden",
        "element": 3,
        "reason": "布局不正常，导致内容被隐藏"
    },
  ]
}

## 指示
# 观察图片中并查找出文本溢出，相互重叠，内容被隐藏，内容非正常换行，存在中文，内容省略等问题。
# 在 json markdown 代码块中输出回答。

```

