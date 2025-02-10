---
title: Using markdown for CVs
date: 2025-02-10 19:19:00 +/-0000
categories: [notes]
tags: [css, markdown, html]     # TAG names should always be lowercase
description: Converting a markdown CV into PDF using Pandoc and Weasyprint
toc: false
comments: false
pin: false
#media_subpath: /path/to/media/
---

I rarely use Word for anything these days. When very occasionally I do need something in `.docx` or `.pdf` format, the thing I enjoy least is messing around with the formatting in Word to make it look presentable.

CVs are the worst for this. Fixed-term employment means I need to apply for new roles fairly regularly, so I wanted to figure out an efficient workflow for outputting a presentable CV and covering letter in PDF format from markdown files.

The following is what I came up with. It's not the most streamlined process, but (unusually for me) I'm happy with this not being more automated.

## Tools

**A Linux environment and markdown editor**: I built upon an existing VSCode devcontainer I'd created for other Pandoc tasks. 

**[Pandoc](https://pandoc.org/)**: This is a super-useful tool for converting files to/from different markdown formats. Depending on your flavour of linux, this can be installed with:

```sh
sudo apt install pandoc
```

I also installed `texlive-latex-recommended`

```bash
sudo apt install texlive-latex-recommended
```
and fonts:

```sh
sudo apt install texlive-fonts-recommended

sudo apt install texlive-fonts-extra
```
This is because it seemed that Pandoc would [need a LaTeX engine](https://pandoc.org/chunkedhtml-demo/2.4-creating-a-pdf.html) to ouput a PDF. I need to investigate this further as I'm not sure all these are needed if using WeasyPrint.

**[WeasyPrint](https://github.com/Kozea/WeasyPrint)**: An HTML and CSS rendering engine that can output to PDF.

WeasyPrint is currently packaged for Debian 11 or newer, and can be installed with:

```sh
sudo apt install weasyprint
```
For other Linux flavours, installation is a bit more involved, as described in the [WeasyPrint installation docs](https://doc.courtbouillon.org/weasyprint/v52.5/install.html). Looks to be a matter of installing dependencies separately using pip.

## Workflow

I created two template files: A markdown file for the content, and a CSS stylesheet.

I also have a big markdown file containing all the details of my employment history, qualifications, CPD projects and courses. This is structured like a CV but far too long to work as one. 

When I need to generate a new CV, I create a new directory in my git repo and add copies of the CSS and markdown template files. I copy/paste the relevant bits of content from the big reference markdown file, and when I'm happy with the content I generate the PDF:

In the terminal, cd into the directory with the files then run the following command:

```
pandoc cv.md -o cv.pdf --pdf-engine=weasyprint --css cv.css
```

where `cv.md` is the markdown file with the CV content, `cv.css` is the stylesheet, and `cv.pdf` is the target file that will be generated in the same subdir.

## Template Files

My markdown template looks something like the example below. Things like job titles, employment dates, qualifications are always the same as the template, unless I need to add a new job/qualification, so all I need to do is edit the content to add/remove details to tailor the CV to the role a bit.

```markdown

<div class="heading">
  <h1>My Name</h1>
  <p class="info">0131 123456 | my_email@address.com | Location </p>
</div>

## Professional Summary

Some words about me and my work

---

## Skills

<ul class="column-list">
  <li>skill1</li>
  <li>skill2</li>
  <li>skill3</li>
  <li>skill4</li>
</ul>

---

## Employment History

### Job Title - Company
#### _Date from - Date to, Location_
- Some 
- things
- I 
- did

### Job Title - Company
#### _Date from - Date to, Location_
- Some 
- things
- I 
- did

---

## Qualifications
- **Qual1** - Institution - Date
- **Qual2** - Institution - Date
- **Qual3** - Institution - Date

---

## CPD
- Some
- things
- I
- did

```

The CSS file is also very simple:

```css
@page {
  margin: 6mm; /* Set page margin in mm */

}

html {
font-size: 10pt; /* Set the base font size */
}

/* Define format for heading */
.heading {
  display: flex;
  justify-content: space-between;
  align-items: baseline; 
  border-bottom: 1px solid #333;
  margin-bottom: 1rem;

}

/* Define format for contact info */
.heading .info {
  margin: 0; 
  text-align: right; 
  font-size: 1rem; 
}

/* Define format for skills list; split across two columns */
.column-list {
  column-count: 2; 
}

/* Define format for body text */
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 1rem; /* 1rem = 10pt */
  line-height: 1.25;
  color: #24292e;
  background-color: #fff;
  padding: 1.25rem;
}

a {
  color: #0366d6;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

ul, ol {
  padding-left: 1em; 
}


li {
  margin-top: 0.0em;
}

p, li {
  margin-top: 0;
  line-height: 1.6;
}

/* Define format for Name */
h1 {
  color: #333;
  margin: 0;
  margin-bottom: 0.5rem;
  font-size: 2rem;
}

/* Main section headings: Employment history etc. */
h2 {

  color: #333;
  font-size: 1.3rem;
  margin-top: 0;
  margin-bottom: 0.5rem;
}


/* Job titles */
h3 {
  color: #333;
  font-size: 1.0rem;
  margin-top: 0;
  margin-bottom: 0;
}


/* Job subheading (dates, location) */
h4 {
  color: #555;
  font-size: 0.75rem;
  margin-top: 0;
  margin-bottom: 0.5rem;
}


/* Other job titles */
h5 {
  color: #555;
  font-size: 0.8rem;
  margin-top: 0;
  margin-bottom: 0;
}

```
This produces a CV in PDF format with a layout I'm happy with, and I get to work in markdown and keep track of everything in git.

Of course, you could automate this workflow further and add API calls to an AI agent, to e.g., insert relevant content based on the big reference markdown file, or review the CV in relation to the job spec. It would be interesting to experiment with that, but its not something I really want for my own purposes right now.