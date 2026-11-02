# Installation Guide — n8n HTTP Request & HTML Extraction Demo

## 1. Introduction

This guide explains how to import, configure, and test the HTTP Request and HTML Extraction demonstration in n8n.

The project retrieves publicly available book information from [Books to Scrape](https://books.toscrape.com/), extracts titles and links, transforms the data, and demonstrates conditional routing using Filter, IF, and Switch nodes.

No paid API or OpenAI API key is required.

## 2. Prerequisites

Before starting, ensure you have:

- Access to n8n Cloud or a self-hosted n8n installation.
- A working internet connection.
- A web browser.
- Basic familiarity with the n8n workflow editor.
- The workflow JSON file from this repository.

## 3. Download the Workflow

1. Open this GitHub repository.
2. Locate `HTTP-Request-Books-To-Scrape-Demo.json`.
3. Open the file.
4. Click **Download raw file** to save the JSON to your computer.

## 4. Import the Workflow into n8n

1. Sign in to n8n.
2. Create a new workflow.
3. Open the workflow's three-dot menu.
4. Select **Import from File**.
5. Choose the downloaded JSON file.
6. Confirm that the imported nodes and their connections appear in the editor.
7. Save the imported workflow.

The imported workflow should contain nine nodes:

1. Manual Trigger
2. HTTP Request
3. HTML
4. Code (JavaScript)
5. Edit Fields
6. Split Out
7. Filter
8. IF
9. Switch

The Filter, IF, and Switch nodes should be connected as separate branches from Split Out.

## 5. Verify the HTTP Request Node

Open the HTTP Request node and confirm:

| Setting | Value |
|---|---|
| Method | GET |
| URL | `https://books.toscrape.com/` |
| Authentication | None |

Execute the node.

**Expected result:** One output item containing the website's HTML content in the `data` field.

## 6. Verify the HTML Extraction Node

Confirm that the HTML node reads from the JSON property `data`.

The extraction configuration should include:

| Output Key | CSS Selector | Attribute | Return Array |
|---|---|---|---|
| bookTitles | `article.product_pod h3 a` | `title` | Enabled |
| bookLinks | `article.product_pod h3 a` | `href` | Enabled |

Execute the node.

**Expected result:** Arrays of book titles and relative book links.

## 7. Verify the JavaScript Transformation

The Code node combines the extracted titles and links into structured book records.

It also converts relative links into complete URLs using the website's base address.

**Expected output structure:**

```json
{
  "totalBooks": 20,
  "books": [
    {
      "title": "Example Book",
      "url": "https://books.toscrape.com/catalogue/example/index.html"
    }
  ]
}
```

The example above illustrates the structure; the actual output contains the extracted book records.

## 8. Verify Edit Fields and Split Out

### Edit Fields

Confirm that:

- The field name is `bookRecords`.
- The field type is Array.
- The field value is the expression `{{ $json.books }}`.
- Include Other Input Fields is disabled.

### Split Out

Confirm that:

- Fields to Split Out is `bookRecords`.
- Include Other Fields is disabled.

Execute Split Out.

**Expected result:** 20 individual items, each containing a book title and URL.

## 9. Test the Filter Node

Configure or verify:

- Value 1: `{{ $json.title }}`
- Operator: String → contains
- Value 2: `the`
- Ignore Case: Enabled

**Observed test result:**

- 10 items kept.
- 10 items discarded.

## 10. Test the IF Node

Use the same title condition as the Filter node.

**Observed test result:**

- True output: 10 items.
- False output: 10 items.

Unlike Filter, IF provides separate branches for matching and non-matching records.

## 11. Test the Switch Node

Configure the Switch node in Rules mode.

| Rule | Condition | Output Name |
|---|---|---|
| 1 | Title contains `the` | Contains-The |
| 2 | Title contains `a` | Contains-A |
| 3 | Title contains `of` | Contains-Of |

Enable case-insensitive matching for all three rules.

Set the Fallback Output to **Extra Output**.

Ensure that sending data to all matching outputs is disabled, so each book is routed to its first matching rule.

**Observed test results:**

| Output | Items |
|---|---:|
| Contains-The | 10 |
| Contains-A | 6 |
| Contains-Of | 0 |
| Fallback | 4 |
| **Total** | **20** |

A zero-item output is not necessarily an error. Earlier routing rules may already have matched those records.

## 12. Run the Complete Workflow

1. Return to the workflow canvas.
2. Click **Execute Workflow**.
3. Wait for execution to complete.
4. Inspect the output of each node.
5. Verify that the Split Out node produces 20 book records.
6. Check the Filter, IF, and Switch branches.

The results above were obtained during development and testing. Actual counts may change if the source website's catalogue changes.

## 13. Troubleshooting

### HTTP Request fails

Check your internet connection and confirm that the website is accessible.

### HTML node returns empty arrays

Verify the input property `data`, the CSS selector, and the selected HTML attributes.

### Code node returns no books

Confirm that the extracted array names are `bookTitles` and `bookLinks`.

### Split Out returns one item

Ensure the `bookRecords` field is an actual Array and the Split Out node references that field.

### Filter returns unexpected results

Check the search text, the Expression/Fixed settings, and Ignore Case. Do not include an unintended `/` before `the`.

### Switch outputs unexpected counts

Check rule order, case-insensitive matching, fallback configuration, and whether multiple matching outputs are enabled.

## 14. Responsible Usage

Books to Scrape is provided for practising web scraping.

When applying similar techniques to other websites, review their access policies and applicable laws. Avoid excessive requests, bypassing access controls, or collecting personal data without a lawful basis.

## 15. Conclusion

This demonstration provides practical experience with HTTP requests, HTML extraction, JavaScript transformations, JSON arrays, data filtering, and conditional routing in n8n.

It can be extended later with pagination, Google Sheets logging, scheduling, and error handling.
