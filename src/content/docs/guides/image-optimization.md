---
title: Image Optimization
---

We use the [`astro-imagetools`](https://github.com/RafidMuhymin/astro-imagetools) integration to optimize and generate responsive images. The global configurations for the integration are defined in the `astro-imagetools.config.mjs` file.

`astro-imagetools` exports four Astro components, `Img`, `Picture`, `BackgroundImage`, & `BackgroundPicture`, and five API functions, `renderImg`, `renderPicture`, `renderBackgroundImage`, `renderBackgroundPicture`, & `importRemoteImage`. We use the `Picture` & `BackgroundPicture` components and `renderPicture` & `importRemoteImage` APIs among them. We have yet to use the other components and APIs.

To know more about the available features of `astro-imagetools` and how to use them, check out the [documentation](https://astro-imagetools-docs.vercel.app/).

### Example

```astro
---
import { Picture } from "astro-imagetools/components";

const { page } = Astro.props,
  { ASSETS_URL } = import.meta.env;
---

<Picture
  layout="fill"
  preload="avif"
  breakpoints={{ minWidth: 640 }}
  sizes="clamp(330px, 26.4vw + 150px, 560px)"
  alt={page.Intro_blob.data.attributes.alternativeText}
  src={ASSETS_URL + page.Intro_blob.data.attributes.url}
  attributes={{ link: { media: "(min-width: 640px)" } }}
/>
```

## Content Images

We get content in markdown format from the CMS. Then we render it to HTML on our end. So, it's not possible to directly access the images used inside the content. To optimize the content images, we have to use the [`OptimizeContentImages`](/src/components/OptimizeContentImages.astro) custom component.

```astro
---
import Markdown from "@astrojs/markdown-component";
import Chapterize from "@components/Chapterize.astro";
import OptimizeContentImages from "@components/OptimizeContentImages.astro";

const { page } = Astro.props;
---

<div class="prose content-text">
  <Chapterize>
    <OptimizeContentImages>
      <Markdown>{page.Block_text}</Markdown>
    </OptimizeContentImages>
  </Chapterize>
</div>
```

## Clipped Images

If you want to want to optimize an image that has to be clipped also, use the `ClippedPicture` component instead of the `Picture` component. The [`ClippedPicture`](/src/components/ClippedPicture.astro) component is a wrapper around the `Picture` component. Check out the [`ClippedPicture` component documentation](/docs/tutorials/how-to-clip-images.md#clippedpicture-component) for more information on how to use it.

## Placeholder Images

We show placeholder images while the original images are being loaded so that the user doesn't experience **CLS (Cumulative Layout Shift)** or see blank spaces in the page. `astro-imagetools` has built-in support for generating placeholder images. It's able to generate three types of placeholder images:

- **Blurred**: A 20px version of the original image.
- **Dominant Color**: The dominant color of the original image.
- **Traced SVG**: A traced SVG version of the original image.

The **Dominant Color** option is set as the default placeholder image type in the `astro-imagetools.config.mjs` file. But for transparent images, we use the **Blurred** option because otherwise, the transparent background gets filled with the dominant color.

To know more about the placeholder functionality of `astro-imagetools` and its usage, check out the [`documentation`](https://astro-imagetools-docs.vercel.app/en/components/Picture#placeholder).

## Remote Images

`astro-imagetools` has built-in support for remote images. To know more about how to use remote images, check out the [How to work with remote images](/docs/tutorials/how-to-work-with-remote-images) tutorial.

## Caching

`astro-imagetools` has built-in support for caching to reduce build time for subsequent builds. We have set the cache directory to `.cache/astro-imagetools`. Due to a bug (`Segmentation fault` errors), we frequently used to face in **Cloudflare Pages**, we commit the cache directory to Git so that Cloudflare doesn't need to perform any kind of image optimization related tasks during build.

Whenever any new images get optimized, the cache directory gets updated. But don't commit the cache directory whenever it changes. Commit it only after finishing a PR that contains a large number of changes or image optimization related changes.

Before committing the cache directory, first, delete the entire `.cache/astro-imagetools` directory, and then rebuild the cache from scratch by running the build command. Then commit the cache directory. Otherwise, the cache directory will contain unnecessary images, gradually increasing the size of the repository.

## The `sizes` attribute

The `sizes` attribute is an attribute used by browsers to calculate the exact rendered width of an image before downloading and rendering it. It's used to determine the best image to download from the `srcset` attribute. To know more about the `sizes` attribute, check out the [How to calculate the value of the `sizes` attribute](/docs/tutorials/how-to-calculate-the-value-of-the-sizes-attribute) tutorial.
