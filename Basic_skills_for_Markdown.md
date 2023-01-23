# Notes for Markdown

---

## Titles

- Use "#" to mark the heading level  
  
  For example: Heading level 1 `# Title`.  
  
  This method is available for heading level up to 6.  

- Add many "==" to mark the heading level 1 and many "--" to mark the heading level 2  

## Paragraph

- Use at least one blank line to separate the paragraphs. Do not put spacers or tabs in front of the paragraphs.  
  
  ```
  This is the first paragraph
  
  This is the second paragraph
  ```

## Line break

- Use trailing whitespace (add two or more spacers at the end of one line) to break the lines (换行)  
  
  ```
  This is the first line  
  This is the second line
  ```

## Emphasis

- **Bold** add "**" in the front and back of the words   

```
It is the **Bold Text**.
```

- *Italic* add "*" in the front and back of the words  

```
It is the *Italic Text*.
```

- ***Bold and Italic*** add "***" in the front and back of the words  

```
It is the ***Bold and Italic Text***
```

## Reference

- To refer to a paragraph, add ">" befor it.  

- To refer to many paragraphs, add ">" to all of them.   

- To nested the reference, add ">>"

```
>Strive for success, but do not expect success. By Faraday
```

> Strive for success, but do not expect success. By Faraday  

## List

- Ordered list  

```
1. First item
2. Second item
3. Third item
    1. First item
    2. Second item
```

> For Example:
> 
> 1. Bacteria
> 
> 2. Animals
> 
> 3. Plants
>    
>    1. Gymnospermae
>    
>    2. Angiospermae

- Unordered list  

```
- First item
- Second item
- Third item
    - First item
    - Second item
```

> For Example:
> 
> - Red
> 
> - White
> 
> - Blue
>   
>   - Lightskyblue
>   
>   - Dodgerblue

## Code

- `Code` Add "`" in the front and back to the words

- `Code Block` Add "```" in the front and back line of the paragraph

```
This is a fenced code blocks.
```

## Horizontal rule

- In a independent line, put "---", "***" or "___"

---

***

___

- To put a blank line before and after a horizontal rule, other wise above the horizontal rule, there would be a heading.

## Hyperlink

- The MarkText is download from [MarkText](https://github.com/marktext/marktext "MarkText: a simple and elegant markdown editor")

```
The MarkText is download from [MarkText](https://github.com/marktext/marktext "MarkText: a simple and elegant markdown editor")
```

```
The Mark Text is down load from [MarkText] [1]
[1]: https://github.com/marktext/marktext


In this method, the [1] can be replace by [number] or [letter], 
and spacers and punctuation are allowed in the [labels].
```

## Figures

- The logo of MarkText

- ![MarkText](http://pic.3h3.com/up/2022/0915/20220915084932655.png "The logo of Mark Text")

```
![Name of the Figure](Link "Title of the Figure")
```
