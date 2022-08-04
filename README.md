# Downhill MTB Rider Identification Program

Use this app to be able to determine who you're watching in almost no time.

## This is currently very much proof-of-concept

I threw together this app in a few hours because it was Monday and I had a busy week ahead of me and wanted to use it on Thursday. There is a live [demo site](https://dh-plates.jeffdev.ca).

The data was scraped from the UCI official site, which is annoying because for some stupid reason they only offer it (at least to the public) as a pdf. It is currently hard-coded into `$lib/data.ts` as an array of javacript objects.

## TODO

- Create a database to house and organize the data.
- Create a typescript/Express backend API just for the experience of doing so.
- Automate the scraping of aforementioned PDFs
- Add ability to choose course/year for posterity.

## Feedback Welcome

This whole thing was primarily made for myself as a coding project, but I'm intereted to hear if other people had ideas for futures that might be useful. Feel free to track down my email address (pretty easy) or open an issue and we'll go from there.

Enjoy watching the races, go ride. Thanks to Red Bull for the great work on the live race feeds!