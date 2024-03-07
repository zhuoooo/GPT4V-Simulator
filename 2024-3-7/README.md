### 测试对象

![](./test-object.png)

### 测试目标

识别出对象中所有节点都存在中文

### 测试结果

结果不符合预期，暂时抛开其他类型的识别，单看中文识别准确率不到 50%。

```json
{
  "briefExplanation": "The image shows a part of a webpage with multiple UI issues such as text overflow, content being hidden, and abnormal text wrapping.",
  "bug": [
    {
      "type": "overflow",
      "element": [13, 14, 15, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45],
      "reason": "These elements' contents exceed their container bounds."
    },
    {
      "type": "ellipsis",
      "element": [29, 30],
      "reason": "These elements' text is truncated with an ellipsis."
    },
    {
      "type": "newline",
      "element": [26, 27, 28],
      "reason": "These elements have abnormal text wrapping."
    },
    {
      "type": "block",
      "element": [20, 21, 22, 23, 24, 25],
      "reason": "These elements are blocked by other elements, obstructing view."
    },
    {
      "type": "hidden",
      "element": [16, 17, 18, 19],
      "reason": "These elements' content is not fully visible, indicating a layout issue where content might be hidden."
    },
    {
      "type": "exit-in-chinese",
      "element": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      "reason": "These elements contain Chinese text."
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
# 你是一个专业的视觉设计师，现在需要你对图片中的页面布局进行一个检查。
# 图片是属于网页的一部分。
# 将统计到的问题根据 type 归类。
# 在 json markdown 代码块中输出回答。

```

