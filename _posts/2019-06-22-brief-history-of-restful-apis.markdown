---
layout: post
title:  "A brief history of web APIs"
date:   2019-06-22 06:30:00 +1000
categories: product-management apis
excerpt_separator: <!--more-->
---

Looking at Application Program Interfaces from a product management lense requires us to understand its origins. How and why did the RESTful APIs evolve the way they did. 

<!--more-->


## The origin of modern day APIs 

Modern version of APIs were originally a solution to data integration. If you were an enterprise application, you needed to share data with a lot of other systems (downstream systems for processing, financial systems, analytics applications etc). Instead of custom-built-integrations, applications started offering APIs through which the integration would become more structured. Another reason why integration took centre stage was the emergence of micro-services replacing monolithic application as the de-facto way of building application. 

Access to APIs was initially restricted and providers would practice "extreme vetting" before allowing access to their APIs. However, around the same time some visionary companies looked at APIs from a much broader perspective. Jeff Bezos' legendary API mandate dates back to 2002. It took them another 4 years to realise the immense potential of these APIs through the launch of AWS S3 in March 2006 and AWS EC2 in August 2006. 

## APIs as the enabler

By 2006, the Silicon Valley breed of companies had already launched their developer APIs. The reasoning at that time was to see what could come out of such an ecosystem. They wanted to see if 3rd party developers could build some of the features that these companies could not prioritise. This was even more important for those operating in mature industries (like CRM, Enterprise Applications etc). In order to compete with the breadth of features that SAP, Siebel etc provided Salesforce.com opened up its force.com platform which would allow 3rd party developers to build and launch applications. That meant Salesforce could depend upon the developer community to build bulk email, deep analytics, dashboarding and other such capabilities and offer it to its customers quickly. What happened shortly was that with more developer interest and the opportunity to tap into the Salesforce.com customer base, a significant number of developers started building on this platform, giving Salesforce.com the edge. 

However, a bulk of businesses were still pretty reluctant to share their customers with developers. The entire screen-scraping capabilities were built around circumventing such companies. Banks weren't sharing their customer's data, hotels weren't sharing their rooms availability and iPhone was not keen to allow 3rd Party to develop applications for their iPhone.

## APIs as the differentiator

With some of the biggest social apps like Twitter, Facebook and others offering APIs, the API economy was already underway. Companies would build entire applications solely on the APIs being offered by an application. Tweet Deck, for example built its business entirely upon twitter APIs. Facebook became a platform of choice for game developers and even GMail opened itself up for developers.  This marriage made in heaven between the platform and the developers continued for another few years until the platforms started tightening their controls over who can use their APIs and how can they use them. Twitter's API and Facebook's journey makes for some very interesting reading on how this love-hate relationship evolved. [A few popular application built on top of twitter APIs were acquired by rivals and twitter had to engage in a bidding war over TweetDeck to prevent it from being acquired by UberMedia](https://techcrunch.com/2011/05/23/twitter-buys-tweetdeck-for-40-million/) 

All in all though, these tech applications benefitted immensely from the developers contribution on to their API platform. 

## API as the aggregator

After about half a dozen years of the API economy, some clear trends emerged. It was the aggregators as opposed to pure-play platforms who benefitted more from this ecosystem. [In fact Ben Johnson of Stratechery paraphrases Bill Gates to explain this distinction](https://stratechery.com/2019/shopify-and-the-power-of-platforms/)

> This is ultimately the most important distinction between platforms and Aggregators: platforms are powerful because they facilitate a relationship between 3rd-party suppliers and end users; Aggregators, on the other hand, intermediate and control it.

Take Youtube as an example. On its own application, YouTube makes far more money from content than the creators of such content do. However, the lure of access-to-thousands, if not millions, of customers had creators continue to publish on these aggregators. In fact Apple App Store, Android Play Store, Amazon Kindle and other such digital aggregators repeatedly leverages this practice t achieve immense growth. 


## API from the regulator

It took a while for the govt to figure out the opportunities that APIs offered. However, this realisation was precipitated by certain events, primary among which was the global financial crisis of 2008. The GDPR and Open Banking regulations forced banks to offer their customer data through APIs. The APIs became a conduit to retrieve (and share) user's own data with the wide world. The regulator enforced APIs are begining to proliferate with governments seeing these as a way to increase competition. x`