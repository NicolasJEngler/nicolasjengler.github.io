---
layout: post
title:  "Creating a custom RSS feed for sites that don't have one"
date:   2023-10-18 01:00:00 -0300
categories: 'fun'
---

One of my 2023 resolutions was to get back into the habit of reading, especially since I decided to stop being a snob and read whatever I truly wanted (even if that was some questionable fanfic-y literature). My go-to website to track the books I delve into is (Goodreads)[https://goodreads.com]. Goodreads has an amazing "news" section that's essentially a blog where they recommend upcoming books and I usually align pretty well with what they promote over there, but the sad news is that they don't offer an RSS feed for their news section.

Ever since I left (most) social media, a lot of the information I consume is through RSS feeds (thanks (NetNewsWire)[https://netnewswire.com/]), so this was an issue. Yes, I could've signed up for email notifications but in all reality I wanted to have all the content centralized in my reader, so that's when I took it into my own hands to offer myself a way to create a custom RSS feed. 

### Prerequisites

Before we start, make sure you have Node.js installed on your machine. If not, you can download it from [Node.js' website](https://nodejs.org/).

## Step 1: Set Up the Project

Create a new Node.js project and install the dependencies we'll be using.

```bash
mkdir goodreads-rss-feed
cd goodreads-rss-feed
npm init -y
npm install express request cheerio rss fs path
```

## Step 2: Write an Express App

Write an Express App to expose an API endpoint to serve the XML output. Create a file named `app.js` and add the initial setup for your Express application.

```javascript
const express = require('express');
const request = require('request');
const cheerio = require('cheerio');
const RSS = require('rss');
const fs = require('fs');
const path = require('path');

const app = express();
const port = process.env.PORT || 3000;
```

## Step 3: Function to Create RSS Feed

Define a function `createRSSFeed` that will create the RSS feed by scraping Goodreads content. **IMPORTANT**: this approach could be used for virtually any website that has a sort-of defined HTML structure to offer a list of latest blog posts.

```javascript
function createRSSFeed(url, feedTitle, feedDescription) {
  return new Promise((resolve, reject) => {
    request(url, (error, response, html) => {
      if (error || response.statusCode !== 200) {
        reject(error || `Request failed with status code ${response.statusCode}`);
        return;
      }

      const $ = cheerio.load(html);
      const feed = new RSS({
        title: feedTitle,
        description: feedDescription,
        feed_url: `${url}/rss.xml`,
        site_url: url,
      });

      // This chunk of code is based on Goodreads current HTML structure to show their latest blog posts
      // If you're using this for a different website, you'll need to inspect their markup and adjust this accordingly
      $('.editorialCard').each((index, element) => {
        const title = $(element).find('.editorialCard__title').text();
        const link = $(element).find('.editorialCard__title .gr-hyperlink').attr('href');
        const description = $(element).find('.editorialCard__body').text();
        const pubDate = $(element).find('.editorialCard__timestamp').text();

        feed.item({
          title: title,
          description: description,
          url: link,
          date: new Date(pubDate),
        });
      });
      
      const xml = feed.xml({ indent: true });
      resolve(xml);
    });
  });
}
```

## Step 4: Express Route for RSS Feed

Define an Express route `/api/rss` that triggers the creation of the RSS feed when accessed and serves an XML output for your reader of choice to feed upon. **IMPORTANT**: I decided to use Vercel to host this API, which is why my route is preceded by `/api`, but this isn't mandatory and could be adjusted based on how you'll be hosting the application.

```javascript
app.get('/api/rss', async (req, res) => {
  try {
    const feed = await createRSSFeed(
      'https://www.goodreads.com/news',
      'Goodreads RSS Feed',
      'Latest articles from Goodreads'
    );

    res.header('Content-Type', 'application/xml');
    res.send(feed);
  } catch (error) {
    console.error('Error generating or sending RSS feed:', error);
    res.status(500).send('Internal Server Error');
  }
});
```

## Step 5: Start the Server

Start the Express server to make your RSS feed accessible.

```javascript
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});

module.exports = app;
```

## Step 6: Run the Application

Run your application using the following command:

```bash
node app.js
```

**IMPORTANT**: for persistence, you could host this in a service such as Heroku or Vercel, rather than depending on the host machine running the application for availability.

Visit [http://localhost:3000/api/rss](http://localhost:3000/api/rss) in your browser to access the custom Goodreads RSS feed.

That's it! You've successfully created a custom RSS feed for Goodreads using Node.js. You can now integrate this feed into your preferred RSS reader to stay updated with the latest content from Goodreads in a more personalized way.

**NOTE**: Since Goodreads doesn't offer a full date on the posts they list on their website, and the markup doesn't include a full date somewhere either, posts usually show up with weird dates. Similar issues may happen if other data is missing or is incomplete in the website you're looking to create a feed for.