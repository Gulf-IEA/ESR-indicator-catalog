# Purpose

This repository houses all the data, code, and other files needed to populate the indicator catalog for the Gulf IEA Ecosystem Status Reports. Below is the detailed methodology for compiling the catalog.

## Step 1: create repo

Create new repository in the Gulf-IEA organization for the ESR Indicator Catalog. Clone the repository into RStudio on local computer.

## Step 2: Create issue template

Add a new folder to the .github folder called ISSUE_TEMPLATE. Put a YAML file in that folder that has all the entries for the issue template. I originally copied and pasted the NE SOE template and then modified it for our region. Here is the [reference](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository) for creating a custom issue template. This is done, but the template needs to be reviewed and revised after discussion with the IEA team.

**IMPORTANT** Make sure that you create a tag on Github called "submission". This tag needs to be created ahead of time, but once it is there each new issue using the template should automatically be tagged as a submission.

## Step 3: Create Github issues for each indicator

Once the template is complete, we can fill out issues for each indicator. Collaborators can create an issue on their own, or if they don't use Github, they can fill out a google form with the exact same entries and then I can manually copy the info over to Github. The google form needs to be created. If there should be a figure included in the page for a given indicator, the issue can be edited and the figure link manually added.

## Step 4: Pull issues from Github and create .rds files

In the R folder, there are several scripts needed to do this. The main file is build_rmd_from_issue.R. Before running this, we need to modify several functions (parse_issue.R, create_listobject.R, make_rmd.R, pull_all_issues.R, pull_single_issue.R).

For pull_single_issue.R and pull_all_issues.R all we needed to do was change the Github repository the functions point to. That's done.

For the make_rmd.R function, we can ignore (comment out or delete) lines 58-230. This code calls the ecodata function to create plots. If we eventually want to plot figures automatically for the report, we will have to code them here. That will take some time to figure out. For now we can ignore. Each of the remaining sections will also need to be modified based on how the Github issue template is written. This has not been done yet. Same thing with the create_listobject.R function, it needs to be revised to match the issue template. This has not been done yet.

parse_issue.R should theoretically work as is (haven't tried it since it works within the build_rmd_from_issue.R script.

Once all these functions are modified, we can run the build_rmd_from_issue.R script with pullAllIssues=T (should only do this once, since it will re-write all the .rmd files if you run it again). If you need to just update a single indicator entry, edit the issue in Github, then run this script again with pullSingleIssue=T and indicating the issue number.

The result of running build_rmd_from_issue.R will be a bunch of .rmd files (one for each issue submission) in the chapters folder.

## Step 5: Create the catalog

Call each of the .rmd files into a quarto book. Will get to this step eventually.

## 508 Compliance

Apparently when you render a pdf through Posit it is not 508 compliant. Here are some tips:

Creating a PDF that is 508 compliant involves ensuring that the document is accessible to people with disabilities, particularly those who use screen readers or other assistive technologies. When using Quarto, a scientific and technical publishing system, you can make your PDFs more accessible by following these guidelines:

### 1. Use Structured Content

Ensure that your document is well-structured with proper headings, lists, tables, and other semantic elements. This helps screen readers to navigate the document easily.

### 2. Add Alt Text to Images

Provide alternative text descriptions for all images in your document. In Quarto, you can add alt text using the alt attribute:

```
![Description of the image](path/to/image.png){alt="A meaningful description of the image"}
```

### 3. Use Accessible Color Schemes

Ensure that your color schemes provide sufficient contrast and are accessible to those with color blindness. Use tools like the WebAIM contrast checker to verify this.

### 4. Add Metadata

Add proper metadata such as the title, author, and language of the document. This helps assistive technologies to better understand and present your document.

### 5. Tag PDFs

PDF tagging is essential for 508 compliance as it helps screen readers to understand the structure of the document. Ensure that Quarto generates tagged PDFs.

#### Quarto Configuration for PDF Accessibility

While Quarto does not have built-in configurations specifically labeled for 508 compliance, you can follow these general steps to improve accessibility in your LaTeX PDF output:

1.  **Configure YAML Header**: Add metadata and other configurations in your YAML header.

```
---
title: "Your Document Title"
author: "Your Name"
date: "2023-06-13"
format: pdf
lang: en
---
```

2.  **Use LaTeX Packages for Accessibility**: Use LaTeX packages that enhance PDF accessibility, such as 'accessibility' and 'hyperref'. You can include these in a custom LaTeX preamble file.

Create a preamble.tex file with the following content:

```
\usepackage[a-3b]{pdfx} % For PDF/A-3b compliance
\usepackage{hyperref}
\hypersetup{
  pdfauthor={Your Name},
  pdftitle={Your Document Title},
  pdfsubject={Subject of the document},
  pdfkeywords={keyword1, keyword2, keyword3},
  colorlinks=true,
  linkcolor=blue,
  filecolor=magenta,
  urlcolor=cyan,
}
```

Include this preamble in your Quarto document by referencing it in the YAML header:

```
---
title: "Your Document Title"
author: "Your Name"
date: "2023-06-13"
format:
  pdf:
    include-in-header: preamble.tex
---
```

3. **Tagging and Structure**: Ensure that your content uses proper headings, lists, and other structural elements.

```
# Heading 1

## Heading 2

- Item 1
- Item 2

Table:

| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```


4. **Alt Text for Images**:

```
![An accessible description of the image](path/to/image.png){alt="A meaningful description of the image"}
```

5. **Check PDF with Accessibility Tools**: After generating the PDF, use accessibility checkers such as Adobe Acrobat Pro’s accessibility checker or other tools to ensure compliance and identify areas for improvement.

#### Example Quarto Document 
Here’s a complete example combining the above steps:

```
---
title: "Example Accessible PDF"
author: "Your Name"
date: "2023-06-13"
format:
  pdf:
    include-in-header: preamble.tex
lang: en
---

# Introduction

This is an example of creating an accessible PDF with Quarto.

## Section 1

Here is some content.

### Subsection

- List item 1
- List item 2

![Description of the image](path/to/image.png){alt="A meaningful description of the image"}

## Table Example

| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```


_________________________________________________________________________

This repository is a scientific product and is not official communication of the National Oceanic and Atmospheric Administration, or the United States Department of Commerce. All NOAA GitHub project code is provided on an ‘as is’ basis and the user assumes responsibility for its use. Any claims against the Department of Commerce or Department of Commerce bureaus stemming from the use of this GitHub project will be governed by all applicable Federal law. Any reference to specific commercial products, processes, or services by service mark, trademark, manufacturer, or otherwise, does not constitute or imply their endorsement, recommendation or favoring by the Department of Commerce. The Department of Commerce seal and logo, or the seal and logo of a DOC bureau, shall not be used in any manner to imply endorsement of any commercial product or activity by DOC or the United States Government.
