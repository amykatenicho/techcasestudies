---
layout: post
title:  "Power BI Embedded with Hogarth(WPP)"
author: "Amy Nicholson"
author-link: "https://twitter.com/AmyKateNicho"
author-image: "/images/authors/AmyKateNicho.jpg"
date:   12-16-2016
categories: Power BI Embedded
color: "blue"
#image: "{{ site.baseurl }}/images/Amadeus-Logo.png" #should be ~350px tall
excerpt: Microsoft teamed up with Hogarth(WPP) in December 2016 to help them build out a Proof of Concept with Power BI Embedded inside their Zonza Data Assest Management Application.
verticals: Retail, Consumer Products & Services
language: English
---


# Power BI Embedded: Hogarth (WPP) & Microsoft

# Overview #
A growing requirement for Hogarth’s customers was to have more information and insights about the assets they store on Zonza: Data Assest Management platform. As Hogarth has a typical ISV engagement model, Power BI Embedded felt like the perfect solution for embedding data insights into the application they currently provide for their customers. We designed an architecture, and implemented a solution, where Hogarth produces Power BI (PBIX) reports for a customers data, connects them; via direct query; to an Azure SQL Database and pushes the PBIX files into the cloud in a Power BI Embedded Workspace Collection. Inside their Python application, we used the Power BI Embedded REST APIs to create a JSON Web Token (JWT) and embed an IFRAME that displays the Power BI Reports inside an 'Insights' tab in their Zonza application.

## Key Technologies Used ##
* [Power BI Embedded](https://azure.microsoft.com/en-us/services/power-bi-embedded/)
    * [Power BI Embedded REST APIs](https://docs.microsoft.com/en-us/azure/power-bi-embedded/power-bi-embedded-iframe)
* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/)
* Azure SQL Database ([Direct Query connection to PBI Embedded](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-sql-database-with-direct-connect/))
* [Azure Resource Manager Templates](https://docs.microsoft.com/en-gb/azure/azure-resource-manager/resource-group-authoring-templates)

## Project Core Names and Roles ##
* __Microsoft Team__
    * Amy Nicholson – Microsoft Technical Evangelist (Power BI Embedded Ascend+ Technical Owner)
    * Ross Smith - Microsoft Technical Evangelist
    * Paul West - WPP Cloud Solution Architect
    * Gina Dragulin - Director of Audience Evangelism, leading Intelligent Cloud for Ascend+
* __Hogarth (WPP) Team__
    * Jim Mellor – Hogarth Product Director
    * Ben Colenso – Hogarth Senior Product Manager
    * Tom Viner – Hogarth Senior Developer
    * Chris D’Chuna - Hogarth Architect

# Customer profile #

## Hogarth Worldwide (WPP) ##

**URL: [www.hogarthww.com](www.hogarthww.com)**

Hogarth Worldwide is a marketing implementation agency.

They produce advertising and other marketing communications for clients across all media and all languages. Their production expertise coupled with a powerful workflow and asset management technology delivers quality, control and savings for global brands.

They believe that the production, adaptation and supply of advertising materials is a specialist activity that should be carried out by a company that focuses solely on this discipline. They work alongside creative and media agencies to deliver advertising into markets efficiently and cost effectively – with quality enhanced and with all language and cultural challenges managed by experts. 


## Company Location ##

Hogarth have offices all over the world ([List of worldwide offices](http://www.us.hogarthww.com/contact-us/contact-us-contact-us/)), however on this project we worked with a team in the London GPS offices, Soho, London, UK.


## Enterprise Data Assest Management: Zonza Application ##

**URL: [http://www.us.hogarthww.com/technology/technology-enterprise-asset-management/](http://www.us.hogarthww.com/technology/technology-enterprise-asset-management/)**

Zonza is an enterprise DAM product created by Hogarth for it’s customers, mostly global brands, to store and distribute their marketing content. The content Zonza stores is immensely valuable. By storing the high resolution assets of the master materials created for global campaigns and making it available to anyone granted with secure access, Zonza helps drive the cost of production down for Hogarth’s customers. 


# Problem statement #

Zonza has a typical ISV engagement model with it’s clients where they can use the SaaS solution and customers can upload, download, share and tag assets with metadata. 
A growing requirement for Hogarth’s customers was to have more information and insights about the assets they store on Zonza .  For example, a real pain point is the usage of rights-expired media. Brands have a period of time assets can be displayed, before usage rights license expires. Brands may be fined if they publish content that unlicensed or expired. By making these insights accessible to customers, Zonza can truly help solve that problem.

# Solution, Steps, and Delivery 

## What we worked on and what problems this helped solve ##

The main aim of the Ascend+ experience was to take one of Hogarths cutsomer datasets and be able to embed a report into their Zonza application using Power BI Embedded. Their application is written in Python, so the obvious choice was to use the REST APIs for Power BI Embedded instead of the SDKs. The APIs were used to create a Python token service (JWT creation) and then pass on that token to a workspace collection in Azure to call a PBIX file and display it in an 'Insights' tab of their application. This is a great showcase of using the REST API's to embed reports into any application, no matter the programming language.

The PBIX file contained a report displaying visuals to the user, leading them to understand/investigate information about their asset data held within the data management system. The main data point to solve the problem stated above, is to show when assets might be about to expire.

We also explored the Power BI Javascript APIs, for interaction in and out of the IFRAME in the Zonza application. This will allow the customer to filter the report down to a few items and then be able to click them, recieve the JSON request back from the _dataSelected_ event and push this information to their database and retrieve the URL for the chosen asset. Once selected the user will be taken to the page in the application containing that assest to investigate further. Unfortunately we ran into an issue where an item in a table cannot be selected and an event returned from the API, however we created an example of retrieving data from a bar chart in a sample application to show the possibilities.

Also during the Ascend+ project we created a set of web service HTTP requests in Python that will programmatically take care of the setup and requests of the Power BI Embedded service. This will be called from not only the cutsomer application but also the team focused on the 'Admin Power User' who will want to create and upload PBIX files they have created in Power BI Desktop to be displayed in the end user application. All the REST calls were made in Python using the Power BI Embedded REST APIs

![Final Outcome: Power BI Embedded implemented into Zonza Application](/images/2016-12-14-Hogarth-Zonza-PBIE/final-outcome.png)
 
## The Data Pipeline/Overall Architecture ##
The data architecture below shows not only the Power BI Embedded service and the front end application but the whole data cycle from exporting the Zonza application data to an Azure SQL Database through to calling the Python REST functions in customer and admin cases.

## Customer Architecture ##

![Final Customer Architecture of Solution](/images/2016-12-14-Hogarth-Zonza-PBIE/customer_experience.PNG)

## Admin Architecture ##

![Final Admin Architecture of Solution](/images/2016-12-14-Hogarth-Zonza-PBIE/admin_experience.PNG)


## Architecture in Progress during the hack ##

![A working architecture and progress during the hackathon](/images/2016-12-14-Hogarth-Zonza-PBIE/working1.png)

__Customer Journey through Architecture:__

1. Customer logs into Zonza application with Zonza credentials
2. Customer clicks on the 'Insights' tab
3. This makes a request to the Python Token Service to generate a JWT (JSON Web Token)
4. Once created the JWT is passed to a Power BI Embedded service in Azure where the token is accepted/declined
5. If accepted, the PBIX file uploaded to the Power BI Embedded Workspace is displayed in the Zonza application Insights tab
6. The data contained in the report displayed in Zonza is always up to date, as Direct Query functionality to an Azure SQL Database is setup through the REST APIs

__Admin Super User Journey through Architecture:__

1. Log into the Zonza Admin application
2. Upload a new PBIX file via a form upload
3. Upload calls into Python functions, created to programmatically call the REST APIs and create/upload the (binary) PBIX file to a selected Power BI Embedded Workspace collection/Workspace


## Technical details of Implementation ##

* PBIX files created to show the capabilities of cross filtering and charting visuals over tables
* Power BI Embedded REST APIs were used to create a Python Token Service and Python Admin function calls 
    * A JWT is created in the token service that completes the secure handshake with Azure Power BI Embedded service
    
```Python
import time

import jwt


# https://docs.microsoft.com/en-us/azure/power-bi-embedded/power-bi-embedded-app-token-flow
def generate_token(access_key, workspace_id, report_id, workspace_collection):
    """Generate JWT token."""
    # token times
    start = time.time()
    end = start + 60 * 60

    # create tokens
    headers = {
        "typ": "JWT",
        "alg": "HS256",
    }

    payload = {
        "wid": workspace_id,
        "rid": report_id,
        "wcn": workspace_collection,
        "iss": "PowerBISDK",
        "ver": "0.2.0",
        "aud": "https://analysis.windows.net/powerbi/api",
        "nbf": start,
        "exp": end,
    }

    return jwt.encode(key=access_key, headers=headers, payload=payload)

```

* REST functions to add/edit/delete and update settings/functionality in the Power BI Embedded Workspace Collection - used mainly within the admin application
    * ![Sample Python Code for Power BI Embedded REST Implementation](/images/2016-12-14-Hogarth-Zonza-PBIE/pbie_rest.PNG)
* First step was to complete a manual run through of creation of Power BI Embedded workspace collection/workspaces/PBIX Upload and Token Generation to display a sample report in a POC application
    * ![Power BI Embedded Azure Portal UI](/images/2016-12-14-Hogarth-Zonza-PBIE/azureportal_pbie.PNG)
* Setup the use of Direct Query with Azure SQL DB - updating the credentials for the report datasource so data can be displayed. This used the Power BI Embedded REST API functionality

```Python
class Gateway(object):
    def __init__(self, import_, gateway_id, datasource_id):
        self.import_ = import_
        self.gateway_id = gateway_id
        self.datasource_id = datasource_id
        self.session = import_.session
        path = 'gateways/{}/datasources/{}'.format(
            gateway_id,
            datasource_id)
        self.url = urljoin(import_.workspace.url, path)

    def __repr__(self):
        return 'Gateway(import_={!r}, gateway_id={!r}, datasource_id={!r})'.format(
            self.import_, self.gateway_id, self.datasource_id)

    def update_connection_string(self, username, password):
        db_creds = {
            "credentialType": "Basic",
            "basicCredentials": {
                "username": username,
                "password": password,
            }
        }
        res = self.session.patch(self.url, json=db_creds)
        log_http('PATCH', self.url, res)
        res.raise_for_status()

```

* Created an 'Insights' tab in a dev instance of Hogarth's application (Zonza) and embedded a report in an IFRAME
* Experimented with the Javascript API for Embedded and the events recorded, for example using the _dataSelected_ functionality in Javascript to retrieve JSON values about what visual has been selected and the data contained.
    * A brilliant [demo of the Javascript APIs](https://microsoft.github.io/PowerBI-JavaScript/demo/code-demo/index.html#) from the Power BI Embedded team. This resource helped the team experiment quickly with the possible capabilities.

[Placeholder for Video]
 
## Learnings from Microsoft and Hogarth team ##

__Learnings:__

* By walking through in detail the creation of tokens and embeding of the reports through the REST API sample, meant that it was easier to understand the flow of the system and all steps that need to be completed. This is compared to say the SDK which does lots of functionality for you
* The architecture decsions around where to split into Workspace Collections or workspaces for customers were domain/customer specific. This can depend on global regions needed, customer data segmentation and multi-tenancy

__Feedback/Further Investigation:__

The Microsoft and Hogarth team found feedback that has been provided back t the Power BI Embedded team, as well as areas to make further investigation into such as the partiy between REST APIs and SDK as well as the abilities of the new Javascript API for Event Operation _dataSelected_
We also found when designing the solution that it would be useful to be able to set the display name of the workspace to something human readable, or send some JSON after creation to edit the display name of a workspace so that it is easier to reference from the admin site when needed.

# Conclusion #
In conclusion, the two day hackathon was a success. We achieved the goal of embedding a Power BI file into the Hogarth Zonza application using a JWT, as well as so much more around the programmatic access to resources using the Power BI Embedded REST APIs as well as interaction in and out of the report using the new Javascript Embedded APIs.

The whole team gained valuable knowledge about Power BI Embedded and the wider Azure services and this should allow Hogarth to continue this project into production.