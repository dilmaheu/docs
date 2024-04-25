---
title: How to debug a build issue
---

When there is a build issue, it blocks any updates to the website. And the devs might not be always available. So, here are some general steps to debug a build issue.

> **Note:** There's no guarantee that you'll be able to surely fix the build issue following this tutorial.

## 1. Check the build logs

We use [Cloudflare Pages](https://pages.cloudflare.com/) to host the website. But we do builds on [GitHub Actions](https://github.com/features/actions), and then deploy the website to Cloudflare Pages. So, you can check the build logs on the [Actions tab of the dilmahtea.me repository](https://github.com/dilmaheu/dilmahtea.me/actions).

## 2. Find out which build step failed

A GitHub Actions workflow consists of multiple steps. After opening the build logs of a failed workflow, find out which step failed. We have to take decisions based on the failed step.

## 3. If the failed step is `Publish to Cloudflare Pages`

If the step that failed is `Publish to Cloudflare Pages`, then it's probably an issue on Cloudflare's side. Most of the time, retrying the workflow fixes the issue. If it doesn't, then wait some time and retry again later or check the [Cloudflare Status page](https://www.cloudflarestatus.com/).

## 4. If the failed step is anything other than `Build`

If the step that failed is anything other than `Build`, then it's probably an issue with the code. It's best to ask the devs to fix the issue in this case.

## 5. If the failed step is `Build`

If the step that failed is `Build`, then it can be either be an issue with the code or the content. If the build issue arises after a recent push to production, then it's probably an issue with the code. If it arises randomly, then chances are that it's an issue with the content. Try to find out which error is causing the build to fail.

### 5.1. `Failed to fetch data from CMS, nothing found in cache!`

This error means that the build failed because the content couldn't be fetched from the CMS. If it happens, you should also see an object containing multiple details about the error like below:

```bash
[
  {
    message: 'Cannot return null for non-nullable field ComponentSearchEngineMeta.URL_slug.',
    locations: [ [Object] ],
    path: [
      'catalog',    'data',
      'attributes', 'Products',
      82,           'products',
      'data',       0,
      'attributes', 'localizations',
      'data',       1,
      'attributes', 'category_tea_range',
      'data',       'attributes',
      'Meta',       'URL_slug'
    ],
    extensions: { code: 'INTERNAL_SERVER_ERROR', exception: [Object] }
  }
]
```

The `message` property of the object contains the error message. Try to understand what the message means. If you can't understand the message, search online or use [ChatGPT](https://chat.openai.com/).

The `path` property of the object contains the path to the field that caused the error. It provides a complete hierarchy of the field that you can use to find the problematic field in the CMS. In this case, the path starts with `catalog`, so first, go to the `catalog` content type in the CMS.

If you go to the `catalog` content type, you'll see that there's a field named `Products` which is a repeatable component. According to the `path`, we have to look into the entry at index `82` of the `Products` field. Indexing starts from `0`, so the entry at index `82` is the 83rd entry. So, go to the 83rd entry of the `Products` field. After going to the 83rd entry of the `Products` field, go to the **0th**, which means the first, related entry by the `products` field.

You see that the next part of the path `localizations`. It means that the entry itself doesn't have any issue but one of its localizations has an issue. So, go to the **1st**, which means the second or the `Dutch`, localization of the entry.

Then, find out if the `Meta.URL_slug` field of the entry related by the `category_tea_range` field has any issue or empty. In this particular case that happened in the past, the related `category_tea_range` was still in draft and incomplete. That's why it was missing the `Meta.URL_slug` field. So, the solution was to complete & publish the `category_tea_range` entry.

> **Note**: The index of the localization entries are based on the alphabetical order of the locale codes. Currently there are two languages enabled besides **English** which are **German** and **Dutch**, and their locale codes are `de` and `nl` respectively. So, the index `0` is for **German** and `1` is for **Dutch**.

If you don't see any object like this after the mentioned error, then it's probably an issue with the code or a temporary issue. Try retrying the workflow to see if it fixes the issue. Otherwise, it's best to ask the devs.

### 5.2. `Perfect Ceylon Tea (en) is missing 'size'`

Here in this error message, **Perfect Ceylon Tea** is the name of the product and **en** is the locale code. The error message means that the **size** field of the product is missing. This error is thrown also if the **variant** field of the product is missing. To fix this issue, go to the product entry in the CMS in the respective locale, select the appropriate size or variant for the product and save the changes.

### 5.3. `VipsJpeg: Premature end of input file`

If you see this error, or another error that contains the word `Vips` or an image format like `JPEG`, `PNG`, `WEBP`, somewhere in the message, then it's probably a temporary issue with the images. Most of the time, retrying the workflow fixes the issue.

### 5.4. `Failed to load ***/uploads/<image-file-name>; Invalid image format`

This error usually happens after importing changes from the production CMS to the development CMS. It means that the image file is corrupted. From the URL, find out which image is causing the issue.

Then, go to the CMS and download the image. If the format of the image is `webp`, then convert it to `jpg` and replace the original image with it. Do this in both the production and development CMS. If converting the image to `jpg` doesn't fix the issue, then try resizing or compressing the image. If nothing works, then it's best to ask the devs.

Strapi doesn't support automatically updating the references to replaced images. So, you have to manually update the references to the image in the content entries. If new build issues arise after replacing the image, then it's probably because of this.

### 5.5. `Cannot read properties of undefined (reading 'attributes')`

If you see this error or a similar error that starts with `Cannot read properties of undefined` or `Cannot read properties of null`, then try to find out while trying to build which page the error was thrown from. You should see the URL of the previous page just above the error message. It's outputted in the following format:

```bash
├─ /nl/account/congrats/index.html (+60ms)
```

Though there's no direct way to know what the current page is, from the URL previous, you should get an idea what the next page could be or what is its type. If it doesn't help then try to analyze the error to determine what type of page it could be.

The error is unclear about exactly which field is causing the issue. So, just check the content entry/entries to see if there's something wrong with it. If everything seems fine, then most probably it's an issue with the code.

### 5.4. Any other error

If you see any other error, then perform the same steps as mentioned in the [previous section](#53-cannot-read-properties-of-undefined-reading-attributes).
