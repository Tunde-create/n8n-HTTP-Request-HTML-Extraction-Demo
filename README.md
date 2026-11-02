# HTTP Request & HTML Extraction Demo Using n8n

## Project Overview

This project demonstrates how to retrieve publicly available HTML content from a website, extract structured information, transform the extracted data, and route records using different n8n nodes.

The demonstration uses [Books to Scrape](https://books.toscrape.com/), a website designed for practising web scraping.

**Project Type:** AI Automation Training / Practical Learning  
**Platform:** n8n  
**Data Source:** Books to Scrape  
**Records Processed:** 20 books

## Project Objectives

- Understand how HTTP requests retrieve website content.
- Extract book titles and links from HTML.
- Transform extracted data using JavaScript.
- Understand arrays and individual JSON items.
- Demonstrate data filtering and conditional routing.
- Compare Filter, IF, and Switch nodes.
- Test and document workflow execution results.

## Workflow Architecture

The workflow begins with a Manual Trigger, followed by an HTTP Request, HTML extraction, JavaScript transformation, Edit Fields, and Split Out nodes.

Three independent branches from Split Out demonstrate Filter, IF, and Switch operations.

```text
Manual Trigger
      |
HTTP Request (GET)
      |
HTML Extraction
      |
Code (JavaScript)
      |
Edit Fields
      |
Split Out (20 Books)
      |
      +---- Filter
      |
      +---- IF
      |
      +---- Switch
```

## Nodes and Their Functions

| Node | Function |
|---|---|
| Manual Trigger | Starts the workflow manually |
| HTTP Request | Retrieves HTML from Books to Scrape |
| HTML | Extracts book titles and relative links using CSS selectors |
| Code (JavaScript) | Combines titles and links into structured book records |
| Edit Fields | Creates a `bookRecords` array |
| Split Out | Converts the array into 20 individual items |
| Filter | Keeps records whose titles contain `the` |
| IF | Separates records into True and False outputs |
| Switch | Routes records according to multiple title-matching rules |

## Data Extraction

**Website:** https://books.toscrape.com/

**CSS Selector:**

`article.product_pod h3 a`

The HTML node extracts:

- `bookTitles` — an array of book titles.
- `bookLinks` — an array of relative book URLs.

The JavaScript node combines these arrays and constructs complete book URLs.

The resulting data contains a `totalBooks` count and a `books` array.

## Testing and Results

### HTML Extraction and Transformation

Successfully extracted and transformed 20 book records.

### Split Out Node

Successfully converted the array of 20 books into 20 individual n8n items.

### Filter Node

Condition: Book title contains `the`, ignoring case.

- Kept: 10
- Discarded: 10

### IF Node

Condition: Book title contains `the`, ignoring case.

- True: 10
- False: 10

### Switch Node

The Switch node uses three routing conditions and an additional fallback output.

| Output | Condition | Items |
|---|---|---:|
| Contains-The | Title contains `the` | 10 |
| Contains-A | Title contains `a` | 6 |
| Contains-Of | Title contains `of` | 0 |
| Fallback | No rule matched | 4 |
| **Total** | | **20** |

The workflow was configured to route each book to its first matching output.

All 20 records were accounted for without duplication.

## Key Lessons Learned

1. An HTTP Request can retrieve HTML rather than JSON.
2. CSS selectors help extract specific information from HTML.
3. JavaScript can transform extracted arrays into structured objects.
4. Edit Fields can prepare data for downstream nodes.
5. Split Out converts an array into separate workflow items.
6. Filter removes items that fail a condition.
7. IF separates matching and non-matching items into two branches.
8. Switch supports multiple routing rules and fallback handling.
9. Rule order matters when records can satisfy multiple conditions.
10. Testing each node independently makes troubleshooting easier.

## Tools and Technologies

- n8n
- HTTP GET requests
- HTML and CSS selectors
- JavaScript
- JSON
- GitHub

## Workflow File

`HTTP-Request-Books-To-Scrape-Demo.json`

Import this file into n8n to examine and run the demonstration.

## Responsible Web Scraping

This project uses Books to Scrape, a website intentionally provided for web scraping practice.

For other websites, review their terms, robots.txt, applicable laws, and access restrictions before collecting data. Avoid excessive requests and do not collect private information without an appropriate legal basis.

## Project Status

**Completed — Successfully Tested**

All primary workflow nodes were configured and tested, including Filter, IF, and Switch routing.

## Future Improvements

- Extract book prices and ratings.
- Add pagination to retrieve more books.
- Save extracted records to Google Sheets.
- Explore scheduling and error handling.
- Compare HTML extraction with API-based data retrieval.

## Author

Developed as a hands-on n8n automation learning project.
