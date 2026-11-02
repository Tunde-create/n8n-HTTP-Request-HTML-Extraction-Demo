# n8n HTTP Request & HTML Extraction Demo

## Project Overview

This project demonstrates how to retrieve HTML content from a public website, extract useful information, clean and transform data, and route records using different n8n nodes.

The project was developed as a practical learning exercise to understand how APIs, HTTP requests, HTML extraction, JavaScript processing, and conditional routing work in n8n.

**Website used for demonstration:** https://www.nairaland.com/users

**Automation platform:** n8n

**Project status:** Completed and successfully tested.

## Project Objectives

- Understand how HTTP GET requests retrieve website content.
- Extract page titles and hyperlinks from HTML.
- Clean, validate, and deduplicate extracted URLs using JavaScript.
- Understand how Edit Fields modifies data structures.
- Convert arrays into individual items using Split Out.
- Compare Filter, IF, and Switch nodes.
- Test and validate workflow outputs.
- Document the completed workflow for reuse and learning.

## Workflow Architecture

The workflow consists of the following nodes:

```text
Manual Trigger
      |
HTTP Request
      |
HTML
      |
Code in JavaScript
      |
Edit Fields
      |
Split Out
      |
      +------ Filter
      |
      +------ IF
      |
      +------ Switch
```

The Filter, IF, and Switch nodes are connected independently to Split Out so they can process the same input data.

## Node-by-Node Explanation

### 1. Manual Trigger

Starts the workflow manually for testing and demonstration.

### 2. HTTP Request

- Method: GET
- URL: https://www.nairaland.com/users
- Purpose: Retrieve HTML content from the website.

The returned HTML is stored in the `data` field.

### 3. HTML Node

Extracts information from the HTML response.

Two extraction fields were configured:

| Field | CSS Selector | Extracted Data |
|---|---|---|
| pageTitle | `title` | Website page title |
| pageLinks | `a` | Hyperlink `href` attributes |

The `pageLinks` extraction returns an array of links.

### 4. Code in JavaScript

Processes the extracted hyperlinks.

The JavaScript code:

- Removes invalid and empty values.
- Trims unnecessary whitespace.
- Converts relative links to absolute URLs.
- Removes duplicate URLs.
- Counts the cleaned links.

**Test result: 46 unique cleaned URLs.**

### 5. Edit Fields

Demonstrates how to rename and restructure fields.

The original `cleanLinks` array was renamed to `websiteLinks`.

**Test result:** One item containing an array of 46 URLs.

### 6. Split Out

Converts the `websiteLinks` array into individual n8n items.

**Test result:** 46 separate items, each containing one URL.

### 7. Filter

Filters URLs based on a condition.

**Condition:** URL contains `/politics`.

Results:

- Input: 46 items
- Kept: 2 items
- Discarded: 44 items

This demonstrated how the Filter node selects matching records.

### 8. IF

Demonstrates two-way conditional routing using the same `/politics` condition.

Results:

| Branch | Items |
|---|---:|
| True | 2 |
| False | 44 |
| Total | 46 |

This demonstrated how the IF node routes matching and non-matching records through separate outputs.

### 9. Switch

Demonstrates multiple conditional routing outputs.

The following categories were configured:

| Category | Condition | Items |
|---|---|---:|
| Politics | Contains `/politics` | 2 |
| Business | Contains `/business` | 1 |
| Jobs | Contains `/jobs` | 2 |
| Other | Fallback output | 41 |
| **Total** | | **46** |

The Switch node successfully distributed all 46 items across four outputs.

## Filter vs IF vs Switch

| Feature | Filter | IF | Switch |
|---|---|---|---|
| Main purpose | Select records | Two-way routing | Multiple routing |
| Matching records | Passed onward | True branch | Matching rule output |
| Non-matching records | Excluded from main output | False branch | Other matching rule or fallback |
| Typical application | Data selection | Conditional decisions | Categorization |

## Challenges Encountered and Solutions

### Challenge 1: Invalid hyperlinks

The HTML extraction returned a mixture of relative URLs, absolute URLs, empty values, and other link formats.

**Solution:** JavaScript filtering and string operations were used to validate, normalize, and deduplicate the links.

### Challenge 2: URL is not defined

An initial JavaScript approach using the `URL` constructor produced an error in the n8n Code node environment.

**Solution:** Relative URLs were converted into absolute URLs using string concatenation with the website's base address.

### Challenge 3: Understanding Edit Fields vs Split Out

Both nodes were tested using the same array.

**Solution:** Practical execution demonstrated that Edit Fields preserved the array within one item, while Split Out generated 46 individual items.

### Challenge 4: Extra Switch routing rule

An unintended fourth routing rule resulted in an additional output.

**Solution:** The extra rule was removed, leaving three configured routing rules and one Fallback Output.

The workflow was executed again and all 46 items were correctly distributed.

## Key Learning Outcomes

This project improved practical understanding of:

1. HTTP GET requests and HTML responses.
2. CSS selectors and HTML data extraction.
3. JavaScript data cleaning and transformation.
4. Arrays versus individual n8n items.
5. Conditional filtering and routing.
6. Workflow testing and debugging.
7. Exporting n8n workflows as reusable JSON files.
8. Technical documentation and version control using GitHub.

## Installation and Usage

1. Install or access an n8n instance.
2. Download the workflow JSON file from this repository.
3. Open n8n and create a new workflow.
4. Import the downloaded JSON file.
5. Review the HTTP Request URL and node configurations.
6. Execute the workflow manually.
7. Inspect the outputs of the HTML, Code, Edit Fields, Split Out, Filter, IF, and Switch nodes.

**Note:** The workflow depends on the structure and accessibility of the external website. If the website changes its HTML structure or link content, the extraction results may change.

## Screenshots

Screenshots of the workflow, node configurations, and successful execution results will be added to the repository.

## Project Purpose

This project was developed for educational purposes and hands-on AI automation training.

It demonstrates how n8n can retrieve, transform, filter, and route website data using visual workflow nodes and JavaScript.

## Future Improvements

Potential extensions include:

- Saving extracted links to Google Sheets.
- Categorizing links using additional conditions.
- Adding scheduled execution.
- Implementing error handling.
- Creating summary reports from extracted data.

## Conclusion

The workflow successfully retrieved HTML content, extracted and cleaned website hyperlinks, converted an array into individual items, and demonstrated conditional processing using Filter, IF, and Switch nodes.

All major workflow components were tested successfully, and the final workflow was exported for reuse and documentation.
