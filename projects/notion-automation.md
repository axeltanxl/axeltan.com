---
layout: post
title: 'Notion Automation'
---

I use Notion everyday to keep track of my tasks for the day and ensure that I reach my goals. I do this by creating Day Plans everyday to remind myself of my goals and to keep track of my progress. This project allowed me to automate the manual process of creating the Day Plans. Before this, I had to manually duplicate the pages, update the date and find a new Quote of the Day everyday.

The script (coded in Python) uses the Notion API to create the page in Notion at a set time everyday, deployed as a cron job in an AWS EC2 instance.

{% include image.html url="https://github.com/axeltanxl/notion-automation" image="projects/notion-automation/thumbnail.jpg" %}