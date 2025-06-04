# Tree-KG

**Tree-KG** is an expandable framework for constructing and iteratively expanding knowledge graphs (KGs) in knowledge-intensive domains.

## Example Usage

### 1. Submit a Task

```bash
curl -X POST https://pacman.cs.tsinghua.edu.cn/api/treekg/submit_task \
  -F "user_config.json=@path/to/user_config.json" \
  -F "file_dir=@path/to/input_file"
````

Example book:
*《大学物理学（第4版）电磁学、光学、量子物理》*
张三慧 编著 安宇 阮东 李岩松 修订
清华大学出版社

Example `user_config.json`:

```json
{
    "course_name": "Physics",
    "material_name": "Electromagnetic_Optics_Quantum_Physics.pdf",
    "book_start_page": 1,
    "book_end_page": 507,
    "cover_start_page": 1,
    "cover_end_page": 1,
    "toc_start_page": 2,
    "toc_end_page": 9,
    "text_start_page": 10,
    "text_end_page": 479,
    "appendix_start_page": 480,
    "appendix_end_page": 507,
    "toc_max_level": 3,
    "toc_re_expression": [
      "(第\\d+篇.*\\n\\n)",
      "(第\\d+章.*\\n\\n|.*物理趣闻.*\\n\\n|.*元素周期表.*\\n\\n|.*数值表.*\\n\\n|.*部分习题答案.*\\n\\n|.*索引.*\\n\\n)",
      "(\\*?\\d+\\.\\d+.*\\n\\n|.+?\\n\\n)"
    ]
}
```

#### Field Descriptions

* **course\_name**: e.g., `"Physics"`
* **material\_name**: e.g., `"Electromagnetic_Optics_Quantum_Physics.pdf"`
* **Page ranges** [book, cover, TOC (table of contents), text, appendix]: counted from the first file page, not internal book numbering.
* **toc\_max\_level**: maximum TOC depth (starts from 1).
* **toc\_re\_expression**: regular expressions to match TOC levels.


### 2. Check Task Status

```bash
curl https://pacman.cs.tsinghua.edu.cn/api/treekg/task_status/${task_id}
```

Example response:

```json
{
  "task_id": "uuid-string",
  "status": "processing",
  "progress": 60,
  "current_step": "augmentation",
  "created_at": "2025-05-30T12:00:00",
  "updated_at": "2025-05-30T12:05:00"
}
```

### 3. Retrieve Task Results

```bash
curl -O https://pacman.cs.tsinghua.edu.cn/api/treekg/task_result/${task_id}
```