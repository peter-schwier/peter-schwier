# GitHub - ardislu/sheets-db: Minimal Google Apps Script (GAS) project intended to quickly transform a Google Sheet into a SQLite database.

markdownload-timestamp:: 2023-04-14T09:14:42 (UTC -05:00)
markdownload-source:: https://github.com/ardislu/sheets-db
markdownload-hostname:: github.com
author:: ardislu
tags:: Google Apps Script, Sqlite, Google Sheet



## Excerpt
> Minimal Google Apps Script (GAS) project intended to quickly transform a Google Sheet into a SQLite database. - sheets-db/sidebar.html at main · ardislu/sheets-db

---
## sheets-db

[![sheets-db logo][fig1]](https://github.com/ardislu/sheets-db/blob/main/logo.webp)

Minimal [Google Apps Script (GAS)](https://developers.google.com/apps-script) project intended to quickly transform a Google Sheet into a SQLite database.

[Click here to create a copy of the demo Google Sheet (the required GAS code will also be copied)](https://docs.google.com/spreadsheets/d/1pY-0oDh4pI8NffZQwN-AkpfJdot7bmlM9nky7w150UY/copy).

## Requirements

-   A Google Sheet to attach this Google Apps Script project to

## Initial setup

These steps only need to be completed once.

## Setting up the Google Sheet

1.  Create a new Google Sheet or create a copy of the [demo Google Sheet](https://docs.google.com/spreadsheets/d/1pY-0oDh4pI8NffZQwN-AkpfJdot7bmlM9nky7w150UY/copy).
2.  Go to `Extensions > Apps Script` to create a new Google Apps Script (GAS) project. The GAS project is automatically attached to the Google Sheet.
3.  Copy `Code.gs` and `sidebar.html` into the GAS project and save. If you copied the demo Google Sheet, this code should already be there.

## (OPTIONAL) Setting up "Upload .sqlite3 file to Backblaze"

If you use [Backblaze](https://www.backblaze.com/) as a storage provider, you can use the "Upload .sqlite3 file to Backblaze" button to upload the SQLite database directly to Backblaze.

This setup is **NOT** necessary if you only want to generate SQLite databases.

4.  In Backblaze, create a new application key with write access to the storage bucket where you want to upload the file to.
5.  Note the `keyID` and `applicationKey` shown.
6.  In the GAS editor, create the following [Google Apps Script properties](https://developers.google.com/apps-script/guides/properties) (environment variables) under `Project Settings > Script Properties`:

-   `BACKBLAZE_ID` - the Backblaze application key ID (`keyId`) from step 5.
-   `BACKBLAZE_KEY` - the Backblaze application key (`applicationKey`) from step 5.

## Usage

1.  Refresh the Google Sheet and wait for the GAS code to finish loading.
2.  See the new custom menu button `🔥 sheets-db`. Press `🔥 sheets-db > ❓ Help` for end-user instructions.
3.  Press `🔥 sheets-db > 🟢 Start` to initialize the SQLite sidebar.
4.  Click the buttons on the sidebar to generate the SQLite database and/or upload the file to Backblaze.
5.  Upload the .sqlite3 file to your favorite SQLite client (example: [sqliteviz](https://lana-k.github.io/sqliteviz/#/workspace)), to run queries on the database.

[fig1]: data:image/webp;base64,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
