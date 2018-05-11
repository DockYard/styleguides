#SEO
## Good reads (found throughout this doc)
* [benackles SEO Guidelines](https://gist.github.com/benackles/0cac8c561423f01a0e52)
* [Keyword Reading](https://yoast.com/meta-keywords/)
* [Schema.org Microdata](http://schema.org/docs/gs.html)
* [Google Structured Data Intro](https://developers.google.com/search/docs/guides/intro-structured-data)
* [Google Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool)
* [WebAiM alt text info](https://webaim.org/techniques/alttext/)
* [Wave Testing Tool Chrome Download](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh)

## Overall approach
### Do whatever is best for human user experience above all else.
As search engine algorithms have evolved over time, they’ve begun to strongly favor sites that simply have well-structured, relevant, and engaging content.

Search engine rankings are speculated to give significant weight to sites with:
* Semantic markup
* Responsive web design
* Strong adherence to accessibility standards
* Keep the page lightweight and mobile performant

Positive, organic user engagement metrics (low bounce rate, time on site, number of pages per session, etc.) also support better page rank as an indicator of good content quality and site credibility.

*Don’t* put too much effort into chasing “advanced” SEO strategies, we can spend our time better buiding a better site which will also help SEO. "Keep it simple"

Many of these efforts produce diminishing returns on time invested, and most meaningful metrics such as site reputation can be improved organically with strong UX and engaging content.

Generally speaking, the overwhelming majority of SEO is determined by factors that are out of the control of web developers—which are mainly meaningful links from external sites, back to the content.
* It's our responsibility to at least inform clients that the # 1 thing they can do to improve their SEO is to get other sites to link to their content—whether it be through social media or dedicated arrangements


## Head related
* Meta title (get as close to *55 characters* as possible, but try to not go over)
* Meta title should include: *Page Name* | *Target Keyword Phrase* | *Brand Name*
* Target keyword phrase is optional if character count allows
* Meta description (get as close to *155 characters* as possible, but try not to go over)
* Meta description should repeat same *page name*, *target keyword phrase*, and *brand name* in well-crafted narrative copy that *accurately describes the page content* and encourages users to click the link to learn more or take action.
* *Don’t* add meta keywords
  * They are not considered necessary and even *considered detrimental by some SEO experts*. They were historically “stuffed” by users who were trying to inorganically get better search rankings by jamming in tons of irrelevant keywords. At best, most search engines don’t give any weight to this content (*most search engines stopped using them for web ranking in 2009*), and some may view it as less credible content.
  * Related article: https://yoast.com/meta-keywords/
STUB Need information on social media meta content and sharing post/preview images

## Page related
* Have a *single*, unique `h1` on every page
  *  `h1` should be the first header tag to appear in the markup structure whenever possible
* Ensure all images have relevant, descriptive alt text when applicable
  * https://webaim.org/techniques/alttext/:
    > If the content that the image conveys is presented within text in the surrounding context of the image, then an empty alt attribute may suffice.
  * Alternative text should not:
    * be redundant (be the same as adjacent or body text).
    * use the phrases "image of…" or "graphic of…".
* Structuring microdata is a significant way to make sure data is appropriately indexed
* Microdata and site maps can be a big deal for indexing—particularly when sites can be *javascript-heavy*
* Use relevant microdata where appropriate to help search engines better understand the meaning of important content
  * Related article: http://schema.org/docs/gs.html
  * Checking tool: https://search.google.com/structured-data/testing-tool

## Other tools and resources
* [WAVE Evaluation Tool - Chrome Web Store](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh)
