---
published: false                        # Optional. Set to true to publish the workshop (default: false)
type: workshop                          # Required.
title: Build Fabric Real-Time Intelligence solution in a day              # Required. Full title of the workshop
short_title: Fabric RTI Tutorial                # Optional. Short title displayed in the header
description: This is a workshop for data enthusiast, Microsoft Fabric learners, data architects, analytics engineers, analysts who are interested in acquiring hands-on skills on Real-Time Intelligence components of Microsoft Fabric.  # Required.
level: intermediate                         # Required. Can be 'beginner', 'intermediate' or 'advanced'
authors:                                # Required. You can add as many authors as needed      
  - Brian Bønk Rueløkke, Devang Shah, Frank Geisler, Johan Ludvig Brattås, Matt Gordon
contacts:                               # Required. Must match the number of authors
  - devsha@microsoft.com
duration_minutes: 360 minutes                    # Required. Estimated duration in minutes
tags: fabric, kql, kusto, eventstream, data activator, real-time dashboard, powerbi          # Required. Tags for filtering and searching
navigation_levels: 2                    # Optional. Number of levels displayed in the side menu (default: 2)
navigation_numbering: true             # Optional. Enable numbering in the side menu (default: true)
#sections_title:                         # Optional. Override titles for each section to be displayed in the side bar
   - Section 1 Introduction
   - Section 2 Fabric Real-Time Intelligence
   - Section 3 The e-commerce store
   - Section 4 Architecture
   - Section 5 Data model
   - Section 6 Pre-requisites
   - Section 7 Building the solution
   - Section 8 Continue your learning 
---

# Introduction

Suppose you own an e-commerce website selling bike accessories. You have millions of visitors a month, you want to analyze the website traffic, consumer patterns and predict sales.  

This workshop will walk you through the process of building an end-to-end [Real-Time Intelligence](<https://blog.fabric.microsoft.com/en-us/blog/introducing-real-time-intelligence-in-microsoft-fabric>) Solution in MS Fabric, using the medallion architecture, for your e-commerce website.  

You will learn how to:
- Build a web traffic analytics solution using Fabric Real-Time Intelligence. 
- **TO BE CHANGED** Use Fabric shortcuts & Data Factory pipelines to get data from operational DBs like SQL Server (with AdventureWorksLT sample data).
- Stream events into Fabric Eventhouse via Eventstream & leverage OneLake availability.
- Create real-time data transformations in Fabric Eventhouse through the power of Kusto Query Language (KQL) & Fabric Copilot.
- Create real-time visualizations using Real-Time Dashboards and automate actions.

See what real customers like [McLaren](<https://www.linkedin.com/posts/shahdevang_if-you-missed-flavien-daussys-story-at-build-activity-7199013652681633792-3hdp>), [Dener Motorsports](<https://customers.microsoft.com/en-us/story/1751743814947802722-dener-motorsport-producose-ltd-azure-service-fabric-other-en-brazil>), [Elcome](<https://customers.microsoft.com/en-us/story/1770346240728000716-elcome-microsoft-copilot-consumer-goods-en-united-arab-emirates>), [Seair Exim Solutions](<https://customers.microsoft.com/en-us/story/1751967961979695913-seair-power-bi-professional-services-en-india>) & [One NZ](<https://customers.microsoft.com/en-us/story/1736247733970863057-onenz-powerbi-telecommunications-en-new-zealand>) are saying.

All the **code** in this tutorial can be found here:   
[Building a Medallion Architecture on Fabric Real-Time Intelligence](<https://github.com/microsoft/FabConRTITutorial/>)  

### Duration
- Lab 2-3 hours (section 8).
- Each section is accompanied with technical explanation of the Fabric Real-Time Intelligence component being used
- **TO BE CHANGED** [pre-reqs](<https://moaw.dev/workshop/?src=gh%3Amicrosoft%2FFabricRTIWorkshop%2Fmain%2Fdocs%2F&step=6>) 30-45 minutes (section 7, recommend provisioning trial tenant prior if necessary).

### Original Creators
This workshop/tutorial was originally written by the following authors and is available at [Fabric-RTI-Workshop]<https://aka.ms/fabricrtiworkshop>
- [Denise Schlesinger](<https://github.com/denisa-ms>), Microsoft, Prin CSA
- [Hiram Fleitas](<https://aka.ms/hiram>), Microsoft, Sr CSA
- Guy Yehudy, Microsoft, Prin PM

### Authors
- [Brian Bønk Rueløkke](<https://www.linkedin.com/in/brianbonk/>), Data Platform MVP
- [Devang Shah](<https://www.linkedin.com/in/shahdevang/>), Principal Program Manager, Microsoft
- [Frank Geisler](<https://www.linkedin.com/in/frank-geisler/>), Data Platform MVP
- [Johan Ludvig Brattås](<https://www.linkedin.com/in/johanludvig/>), Data Platform MVP
- [Matt Gordon](<https://www.linkedin.com/in/sqlatspeed/>), Data Platform MVP

### Feedback - Contributing
- Rate this lab or give us feedback to improve using this short [Eval](<https://forms.office.com/r/xhW3GAtAhi>). Scan this QR Code to open the evaluation form on your phone.

![QR Code](assets/QRCodeLabEval-Small.png "QR Code")

- If you'd like to contribute to this lab, report a bug or issue, please feel free to submit a Pull-Request to the [GitHub repo](<https://github.com/microsoft/FabricRTIWorkshop/>) for us to review or [submit Issues](<https://github.com/microsoft/FabricRTIWorkshop/issues>) you encounter.


---

## Second section

Content for second section
