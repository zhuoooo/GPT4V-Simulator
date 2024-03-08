### 测试对象

![](./test-object.png)

### 视觉标记后

![](./test-object-2.png)

### 测试目标

识别出对象中所有节点都存在中文

### 测试结果

附带了原图的误判率 （2 / 32）。

```json
{
  "briefExplanation": "The page layout contains elements with Chinese text",
  "bug": {
    "type": "exit-in-chinese",
    "element": [1,2,3,4,5,7,8,9,10,11,12,13,14,15,16,17,19,20,21,22,24,25,26,27,29,30,31,32,33], // 6 18 28
    "reason": "The content in these elements is in Chinese"
  }
}

```

未携带原图的误判率 （5 / 32）。

```json
{
  "briefExplanation": "The page contains multiple elements with Chinese text.",
  "bug": [
    {
      "type": "exit-in-chinese",
      "element": [
        1, 4, 5, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
        24, 25, 26, 27, 29, 30, 31, 32, 33, 34
      ], // 2 3 6 23 28 34
      "reason": "These elements contain Chinese characters."
    }
  ]
}

```



### prompt

今日调整：

1. 缩减任务数量减少至“识别中文”
2. 添加原图片，作为在对照图
3. 添 few-shot 案例

```

任务: 查找出存在中文问题。

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
        "type": "exit-in-chinese",
        "element": [1],
        "reason": "这些内容存在中文"
    }
  ]
}

## 指示
# 你是一个专业的视觉设计师，现在需要你检查页面布局。
# 图片一是原图，图片二是添加了辅助视觉标记。
# 注意图片二中带有颜色背景的数字标记并不是页面内容的一部分，而是用来辅助标记的。
# 图片是属于网页的一部分。
# 只提供一个JSON格式的输出，归类并描述这些布局问题。

```

