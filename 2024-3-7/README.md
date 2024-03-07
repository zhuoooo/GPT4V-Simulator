### 测试对象

![](./test-object.png)

### 测试目标

识别出对象中所有节点都存在中文

### 测试结果

结果不符合预期，暂时抛开其他类型的识别，单看中文识别准确率不到 50%。

```json
{
  "briefExplanation": "The webpage has multiple layout issues detected, including overflow, ellipsis, newline, block, hidden, and exit-in-Chinese problems.",
  "bug": [
    {
      "type": "overflow",
      "element": [3, 17, 21],
      "reason": "Content exceeds the boundary of its container."
    },
    {
      "type": "ellipsis",
      "element": [2, 14],
      "reason": "Content is truncated with an ellipsis."
    },
    {
      "type": "newline",
      "element": [6],
      "reason": "Content is breaking into lines abnormally."
    },
    {
      "type": "block",
      "element": [18],
      "reason": "Content is overlapped by other elements."
    },
    {
      "type": "hidden",
      "element": [5, 11],
      "reason": "Content is not fully visible due to being overlapped or off-screen."
    },
    {
      "type": "exit-in-chinese",
      "element": [7, 8, 9, 10, 12, 13, 15, 16, 19, 20, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32],
      "reason": "Content contains Chinese characters."
    }
  ]
}


```

### prompt

```

任务: 查找出文本溢出，相互重叠，内容被隐藏，内容非正常换行，存在中文，内容省略等问题。

## 超出容器
type OverflowAction = { type: "overflow", element: number[], reason: string }
## 出现省略号
type EllipsisAction = { type: "ellipsis", element: number[], reason: string }
## 不正常换行
type NewLineAction = { type: "newline", element: number[], reason: string }
## 内容被遮挡
type BlockAction = { type: "block", element: number[], reason: string }
## 内容被隐藏
type HiddenAction = { type: "hidden", element: number[], reason: string }
## 内容存在中文
type ExitInChineseAction = { type: "exit-in-chinese", element: number[], reason: string }

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
        "type": "overflow",
        "element": [1],
        "reason": "这些内容超出容器范围"
    },
    {
        "type": "ellipsis",
        "element": [2],
        "reason": "这些内容文字出现省略号"
    }
  ]
}
{
  "briefExplanation": "Today's doodle looks interesting, I'll click it",
  "bug": [
    {
        "type": "hidden",
        "element": [3],
        "reason": "这些内容布局不正常，导致被隐藏"
    },
  ]
}

## 指示
# 你是一个专业的视觉设计师，现在需要你检查页面布局。
# 注意图像中带有颜色背景的数字标记并不是页面内容的一部分，而是用来辅助标记的。
# 图片是属于网页的一部分。
# 只提供一个JSON格式的输出，归类并描述这些布局问题。

```

