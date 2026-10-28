# Core GRC consent group triggers for Google Tag Manager

A Google Tag Manager container you import to fire tags only when a visitor allows their [Core GRC](https://core-grc.com) cookie consent group. Use it for tags that don't follow Google Consent Mode, such as Meta Pixel, LinkedIn Insight Tag, TikTok Pixel or Hotjar.

Google tags (Google Analytics, Google Ads, Floodlight) don't need it: they follow Consent Mode, which the [Core GRC Cookie Consent tag template](https://github.com/Core-GRC/gtm-cookie-consent) sets for you.

Setup guide: https://core-grc.com/docs/google-tag-manager/consent-triggers

## What's in the container

[`core-grc-consent-triggers.json`](core-grc-consent-triggers.json) adds 6 triggers and the 6 Data Layer Variables they read, in a folder called **Core GRC consent triggers**. It adds no tags and doesn't change yours.

| Trigger | Fires when this group is allowed | Variable (Data Layer Variable Name) |
| --- | --- | --- |
| Core GRC - Essential granted | Essential (`COR1`). Always allowed. | Core GRC - Essential consent (`categories.COR1`) |
| Core GRC - Functionality granted | Functionality (`COR2`) | Core GRC - Functionality consent (`categories.COR2`) |
| Core GRC - Analytics granted | Analytics (`COR3`) | Core GRC - Analytics consent (`categories.COR3`) |
| Core GRC - Personalization granted | Personalization (`COR4`) | Core GRC - Personalization consent (`categories.COR4`) |
| Core GRC - Advertising granted | Advertising (`COR5`) | Core GRC - Advertising consent (`categories.COR5`) |
| Core GRC - Uncategorized granted | Uncategorized (`COR6`) | Core GRC - Uncategorized consent (`categories.COR6`) |

## How the triggers work

The Core GRC script pushes `core_grc_cookie_consent_updated` to the data layer on every page as soon as it loads (with the visitor's saved choice, or your region's defaults before they choose), and again each time the visitor makes or changes a choice:

```js
{
  event: "core_grc_cookie_consent_updated",
  categories: { COR1: "granted", COR2: "denied", COR3: "granted", COR4: "denied", COR5: "denied", COR6: "denied" },
  consentStatus: "partial",
  source: "user",
  websiteId: "…"
}
```

Each trigger is a **Custom Event** trigger on `core_grc_cookie_consent_updated` with one condition: its group's variable equals `granted`. A trigger never fires for a group your banner doesn't have.

## Install

You need Core GRC Cookie Consent on your website, through the [tag template](https://github.com/Core-GRC/gtm-cookie-consent) or the Core GRC script.

1. Download [`core-grc-consent-triggers.json`](https://raw.githubusercontent.com/Core-GRC/gtm-consent-triggers/main/core-grc-consent-triggers.json). It's also in Core GRC under **Cookie Consent → Websites → your website → Install Script → Google Tag Manager**.
2. In Google Tag Manager, open **Admin → Import Container** and choose the file.
3. Choose your workspace, then **Merge** and **Rename conflicting tags, triggers, and variables**. Don't choose **Overwrite**: it replaces your whole container.
4. Put each tag that isn't from Google on its group's trigger instead of **All Pages**, for example **Core GRC - Advertising granted** for Meta Pixel.
5. On those tags, set **Advanced Settings → Tag firing options** to **Once per page**. A visitor who changes their choice causes a second push.
6. Check it in **Preview**, then **Submit** and **Publish**.

The guide also covers setting the triggers up by hand.

## Support

Open an issue in this repository, or contact Core GRC support. For security problems, see [SECURITY.md](SECURITY.md).

## License

Apache 2.0. See [LICENSE](LICENSE).
