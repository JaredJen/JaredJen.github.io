---
layout: post
title: 使用BeautifulSoup解析HTML/XML
date: 2025-12-28 02:48 +0800
categories: [Python, 网络爬虫]
tags: [Python, 网络爬虫, BeautifulSoup]
---
BeautifulSoup是一个用于解析HTML和XML文档的Python库，它可以帮助你轻松地从网页中提取数据。

## 安装BeautifulSoup

首先，你需要安装BeautifulSoup和解析器：

```bash
pip install beautifulsoup4
pip install lxml  # 推荐安装的解析器，性能更好
# 或者使用Python自带的解析器
# pip install html5lib  # 另一个可选解析器
```

## 基本使用方法

### 1. 导入库并解析文档

```python
from bs4 import BeautifulSoup

# 示例HTML文档
html_doc = """
<html>
<head>
    <title>示例网页</title>
</head>
<body>
    <div id="main">
        <h1 class="title">网页标题</h1>
        <p class="content">第一段内容</p>
        <p class="content">第二段内容</p>
        <a href="https://example.com" id="link1">链接1</a>
        <a href="https://example.org" id="link2">链接2</a>
        <div class="nested">
            <span>嵌套内容</span>
        </div>
    </div>
</body>
</html>
"""

# 创建BeautifulSoup对象
# 第一个参数是要解析的文档，第二个参数是解析器类型
soup = BeautifulSoup(html_doc, 'lxml')  # 或者使用 'html.parser'
```

### 2. 查找元素

```python
# 通过标签名查找第一个匹配的元素
title_tag = soup.title
print(f"标题: {title_tag.string}")

# 查找所有匹配的元素
all_p = soup.find_all('p')
for p in all_p:
    print(f"段落: {p.text}")

# 通过类名查找
contents = soup.find_all(class_='content')
for content in contents:
    print(f"内容: {content.text}")

# 通过ID查找
main_div = soup.find(id='main')
print(f"主div ID: {main_div.get('id')}")

# 通过属性查找
link1 = soup.find(href='https://example.com')
print(f"链接1: {link1.text}")
```

### 3. CSS选择器

```python
# 使用CSS选择器
# 选择所有p标签
all_p = soup.select('p')

# 选择类名为content的元素
content_elements = soup.select('.content')

# 选择ID为main的元素
main_element = soup.select('#main')

# 选择嵌套元素
nested_span = soup.select('div.nested span')
print(f"嵌套span: {nested_span[0].text}")
```

### 4. 获取元素内容和属性

```python
# 获取标签文本内容
h1 = soup.h1
print(f"h1文本: {h1.text}")
print(f"h1文本(另一种方法): {h1.get_text()}")

# 获取标签属性
first_link = soup.a
print(f"第一个链接的href: {first_link.get('href')}")
print(f"第一个链接的id: {first_link['id']}")

# 获取所有属性
print(f"第一个链接的所有属性: {first_link.attrs}")
```

### 5. 导航文档树

```python
# 子节点
body = soup.body
for child in body.children:
    if child.name:  # 过滤掉空白字符等
        print(f"子节点: {child.name}")

# 父节点
h1 = soup.h1
print(f"h1的父节点: {h1.parent.name}")

# 兄弟节点
first_p = soup.find('p')
next_p = first_p.find_next_sibling('p')
print(f"第一个p的下一个兄弟p: {next_p.text}")

# 前后节点
first_a = soup.a
print(f"第一个a后面的元素: {first_a.next_sibling}")
```

## 实际示例：解析网页

```python
import requests
from bs4 import BeautifulSoup

# 获取网页内容
url = 'https://example.com'
response = requests.get(url)
html_content = response.text

# 解析网页
soup = BeautifulSoup(html_content, 'lxml')

# 提取所有链接
all_links = soup.find_all('a')
for link in all_links:
    href = link.get('href')
    text = link.text.strip()
    if href and text:
        print(f"{text}: {href}")

# 提取所有段落文本
all_paragraphs = soup.find_all('p')
for paragraph in all_paragraphs:
    print(paragraph.text.strip())
```

## 处理特殊情况的技巧

```python
# 1. 处理缺失的元素
element = soup.find('nonexistent')
if element:
    print("元素存在")
else:
    print("元素不存在")

# 2. 提取多个属性
links = []
for a in soup.find_all('a'):
    link_info = {
        'text': a.text.strip(),
        'href': a.get('href'),
        'class': a.get('class', [])
    }
    links.append(link_info)

# 3. 清理HTML标签只获取文本
html_with_tags = "<p>这是<b>加粗</b>文本</p>"
clean_soup = BeautifulSoup(html_with_tags, 'lxml')
clean_text = clean_soup.get_text()
print(f"清理后的文本: {clean_text}")

# 4. 处理编码问题
html_bytes = b'<h1>\xe6\xa0\x87\xe9\xa2\x98</h1>'
soup = BeautifulSoup(html_bytes, 'lxml', from_encoding='utf-8')
print(f"解码后的标题: {soup.h1.text}")
```

## 常见问题解决

1. **解析器选择**：
   - `lxml`：速度快，功能强大（推荐）
   - `html.parser`：Python内置，无需额外安装
   - `html5lib`：容错性最好，但速度慢

2. **处理动态内容**：
   BeautifulSoup只能解析静态HTML，对于JavaScript动态加载的内容，需要使用Selenium或Playwright等工具。

3. **性能优化**：
   - 使用`lxml`解析器
   - 避免在循环中重复解析
   - 使用CSS选择器代替复杂的查找链

## 完整示例：爬取新闻标题

```python
import requests
from bs4 import BeautifulSoup

def scrape_news_titles(url, selector='h2'):
    """
    从网页中提取新闻标题
    """
    try:
        # 发送HTTP请求
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
      
        # 解析HTML
        soup = BeautifulSoup(response.text, 'lxml')
      
        # 提取所有标题
        titles = []
        for title in soup.select(selector):
            text = title.text.strip()
            if text:
                titles.append(text)
      
        return titles
  
    except requests.RequestException as e:
        print(f"请求错误: {e}")
        return []
    except Exception as e:
        print(f"解析错误: {e}")
        return []

# 使用示例
if __name__ == "__main__":
    url = "https://news.baidu.com"
    titles = scrape_news_titles(url, '.hotnews a')
    for i, title in enumerate(titles[:10], 1):
        print(f"{i}. {title}")
```

这些是BeautifulSoup的基本用法，掌握了这些你就可以开始解析大多数HTML文档了。根据具体需求，你可能需要组合使用不同的查找方法来实现复杂的数据提取。