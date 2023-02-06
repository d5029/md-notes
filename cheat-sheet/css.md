# CSS小抄
## 任何 HTML 元素都可以通過block的visible/editable；並且內容可編輯 - 包括style標籤！
- 影片：https://www.youtube.com/shorts/D02AK3WoYH8
- html tag加上 `contenteditable` 就能編輯
- 原來 `style` 可以寫在 `body `裡面
```html
<body>
    <style contenteditable style="display: visible;">
        html{
            background-color: red;
        }
    </style>
    <h1 contenteditable>這是標題</h1>
</body>
```