## Introduction
This is a [Craft CMS](https://craftcms.com/) plugin for managing podcasts and episodes.
This plugin is in the development phase so API can be changed.

## Requirement
- Craft 4.4 and higher.

## License & Pricing
This is a commercial plugin available through the [Craft plugin store](https://plugins.craftcms.com).

## Main features:
- Manage multiple podcasts.
- Multi-site support for the podcast and episode elements.
- Each podcast can have a different podcast format, settings, and field layout.
- Bulk import episodes via fetching from RSS and Asset index utility.
- Fetch Id3 metadata (title, duration, year, track number, images, genres) from the episode tracks and save it to custom/native fields
- Generate RSS for podcasts (supported by [Apple](https://help.apple.com/itc/podcasts_connect/#/itcb54353390), [Google](https://support.google.com/podcast-publishers/answer/9889544)).
- GraphQL support for fetching podcasts, and episodes.
- Support for project config.

## Podcast formats
To be able to create a podcast, there must be at least one podcast format.  
To create a podcast format, go to (Plugin settings->Podcast Formats) and click on the new podcast format. Only users with manage plugin settings access can view/create podcast formats.

<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/221650131-d0eae6e8-dc96-4bfc-9954-08c837f621dc.jpg" width="800px">  
</p>  

When creating a podcast format, you may specify these settings for the podcast and its episodes:   
- Name and handle of the podcast format.  
- If the podcast and its episodes support versioning
- Settings for supported sites:  
  - Site: only enabled sites appear on the podcast creation page.
  - Podcast URI Format: for example: "podcasts/{slug}".
  - Template: it is the template path to use when a podcast's URL is requested.
  - Default status: Default status for a podcast when creating a podcast.
  - The podcast and episode field layout.
  - The podcast and episode field mapping -in these tabs, you can specify the custom fields that represent the podcast and its episode attributes.-
  - Extra settings for the podcast, episode native fields - for example, if they support translation or not.

## Podcasts
### Podcast creation:
To create a podcast, the user with permission to manage podcasts/create draft a podcast, can create a podcast/draft podcast.  
When creating a podcast, as default, the only field available is the title field.  Attributes of other native fields available for podcast field layout are:
  - ownerName
  - ownerEmail
  - authorName
  - podcastBlock (as a lightswitch)
  - podcastComplete (as a lightswitch)
  - podcastExplicit (as a lightswitch)
  - poodcastType: Available options are: Episodic and serial
  - podcastLink
  - copyright
  - podcastRedirectTo: if you decided to redirect feed RSS to a new feed, you can set this native field.  
  - podcastIsNewFeedUrl: if this is a new feed URL, and an old feed is redirected to this feed.
  
  You can create custom fields and add them to the field layout via (Plugin settings->Podcast Formats->Podcast field layout).  
  Optionally, for testing purposes, when dev mode is on, you can import sample custom fields quickly via (Plugin settings->Import podcast fields).  
  After creating custom fields, make sure to check the podcast mapping page (Plugin settings->Podcast Formats->Podcast mapping), to specify which custom field is used for the podcast image, podcast description, or podcast category.  
    - if you use a category field for podcast catgeory, make sure 'Maintain hierarchy' option is enabled so sub-categories are fetched correctly.
   
<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/221909217-405423b3-0288-4f19-a731-e8edc98555a2.jpg" width="800px">  
</p>  

### Podcast actions
There are some actions available for each podcast by selecting each podcast and clicking on action button:
<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/232880103-4223abf0-71be-48d1-af23-ce8a693b56a2.png" width="200px">
</p>

#### Genral settings
You can specify these general settings for each podcast per site:
- Publish RSS
- Allow all to see published RSS

#### Episode settings
You can specify these episode settings for each podcast per site. These settings only applies when importing episodes via asset index utility or resaving episodes via console 
- Default image for episodes
- Default pub date for episodes
- How to fetch genre from asset and import it to Craft

#### Import episodes by RSS
You can [import episodes by fetching them from a RSS](https://github.com/vnali/studio-plugin-docs/edit/main/README.md#importing-episodes-via-fetching-data-from-the-rss) via this action. 

#### Import episodes by Asset index
You can specify settings for [importing epiosdes via asset indexes utility](https://github.com/vnali/studio-plugin-docs/edit/main/README.md#importing-episode-via-asset-indexes-utility) via this action.

## Episodes
When creating an episode, as default, the only field available is the title field.
Other native field attributes available for episode field layout are:
- duration (supports both int and HH:MM:SS format)
- episodeBlock (as a lightswitch)
- episodeExplicit (as a lightswitch)
- episodeSeason
- episodeNumber
- episodeType: Available options are: full, trailer, bonus
- episodeGUID: When creating a new episode, GUID is empty. when saving that episode, if GUID is empty, it fills with the element UID.  

You can create custom fields and add them to the field layout via (Plugin settings->Podcast Formats->Episode field layout).  
Optionally, for testing purposes, when dev mode is on, you can import sample custom fields quickly via (Plugin settings->Import episode fields).  

After creating custom fields, make sure to check the episode mapping page (Plugin settings->Podcast Formats->Episode mapping), to specify which custom field is used for the episode file, image, subtitle, summary, description, content encoded, publish date and keywords. 
- On the mapping page there is a special item, 'Map genre metadata to'. You can use this item to get genre metadata from episode's file and save it to a custom field. You can use this custom field later in a template or for example map episode keywords to this custom field to use genre as an episode's keyword.

### Create episodes manually
- Go to the episodes index page.
- Select the podcast you want to create an episode in.
- Add an asset as an episode file.
  - Enable fetch Id3 metadata/fetch Id3 image metadata and click on the fetch button to get title/duration/image/year/genre metadata from the episode file and fill automatically related native/custom fields.
  - Important note: you should specify that fetched metadata is written to which custom field via the episode mapping page (Plugin settings->Podcast Formats->Episode mapping)
- Finally, review and save the episode.

### Importing episode via Asset Indexes utility
This option allows you to create one episode per asset.  
- Upload episode files to the desired volume.
- Go to the podcast index page and click on the desired podcast and from the action list select 'Import episodes by asset index'.
- In "Volumes to import assets as episodes" field, select the volumes you uploaded episodes.
- Make sure "Enable" is checked so episode import is enabled.
- Specify "Sites" that imported epiosde should be saved/propagated to.
  - If more than one site are selected, epiosde settings for first site is used.
- Save this form and use the Asset index utility tool to index assets on those volumes.
  - As default, all volumes are listed on Asset index utility page. On general setting page, there is an option, 'On Asset Indexes utility page, only list the volumes that user has saveAssets:[VolumeUID] permission'. By enabling this option, on the Asset Index Utility page, the user can only view the volumes to which the user has save access.
- Check if assets are imported to Craft and an episode is created for each asset file.
  - Ensure that you specified the episode asset field in the episode mapping page. Otherwise, the plugin can't create an episode from the asset.
- Finally, disable "enable import episodes" to prevent accidental creating episodes when using asset index utility.

### Importing episodes via fetching data from the RSS
- Go to the podcast index page and click on the desired podcast and from the action list select 'Import episodes by URL'.
- In the "Import episode from RSS" field, specify the URL you would like to fetch episodes from it.
- If you would like to limit imported episodes, enter a value for the limit field.
- There are two options "Don't import episode's main asset" and "Don't import episode's image asset" to prevent fetching large files via CURL.
- Specify "Sites" that imported epiosde should be saved/propagated to.
  - If more than one site are selected, epiosde settings for first site is used.
- Save this form.
- The plugin tries to fetch episode contents from that URL and create episodes via a Craft job.

<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/221927757-873abe7e-aea2-4806-8db3-ac3525f3b55a.jpg" width="800px">  
</p>

## Podcast RSS
On the podcast element index page, for an enabled podcast with published RSS, the user -with 'View the podcast' permission- sees a 'View' RSS link.  
If the podcast is disabled or its RSS is not published, that user sees a 'Preview' link instead.  
To publish RSS for a podcast, go to general setting page of that podcast -on podcast index page, select the podcast and use general setting action- and enable 'Publish RSS'

To see A RSS page:
- For a disabled podcast, user must have 'View the podcast' permission.
- For a not published podcast, user must have 'View not published RSS' permission too.
- For a published podcast, user must have 'View published RSS' permission too.
- For a published podcast, to skip permission check, go to general setting page of that podcast and enable 'Allow All to see published RSS'

## Podcast RSS page
Below you can see how `<channel>` and `<item>` tags are created:
- `<channel><title>`/`<channel><itunes:title>`
  - The podcast title is used.
- `<channel><description>`
  - The value of the plain text field/redactor/ckeditor specified on the podcast field mapping page is used.
- `<channel><lastBuildDate>`/`<channel><pubDate>`
  - The most recent updated time for the podcast and its episodes is used
- `<channel><itunes:summary>`
  - The value of the plain text field/redactor/ckeditor specified on the podcast field mapping page is used. 
- `<channel><itunes:image>`
  - The URL of the image custom field specified at podcast field mapping is used.
- `<channel><language>`
  - The current site language is returned.
- `<channel><itunes:category>`
  - The value of the entry/category custom field specified at podcast field mapping is used.
- `<channel><itunes:explicit>`
  - The value of podcastExplicit native field is used.
- `<channel><itunes:author>`
  - The value of the podcastAuthor native field is used.
- `<channel><link>`
  - If the podcastLink native field has value, this value is used. otherwise, the podcast element URL is used.
- `<channel><itunes:owner>`
  - The value of the podcastOwner native field is used.
- `<channel><itunes:type>`
  - The value of the podcastType native field is used.
- `<channel><copyright>`
  - The value of the copyright native field is used.
- `<channel><itunes:new-feed-url>`
  - If the podcastIsNewFeedUrl native light switch field is enabled, the current RSS URL is used.
- `<channel><itunes:block>`
  - The value of the podcastBlock native field is used.
- `<channel><itunes:complete>`
  - The value of podcastComplete native field is used.
- `<item><title>`/`<item><itunes:title>`
  - The episode title is used.
- `<item><itunes:subtitle>`
  - The value of the plain text/redactor/ckeditor field for subtitle, specified in episode field mapping is used.
- `<item><enclosure>`
  - The URL, type, and length of the asset for the specified episode's asset field are used.
- `<item><guid>`
  - The value of the GUID native field is used.
- `<item><pubDate>`
  - The value of the specified date custom field is used. if there is no mapping, episode creates date is used.
- `<item><itunes:summary>`
  - The value of the plain text/redactor/ckeditor field for itunes:summary specified in episode field mapping is used.
- `<item><description>`
  - The value of the plain text/redactor/ckeditor field for description specified in episode field mapping is used.
- `<item><content:encoded>`
  - The value of the plain text/redactor/ckeditor field for content:encoded specified in episode field mapping is used.
- `<item><itunes:duration>`
  - The value of the duration native field is used.
- `<item><link>`
  - The episode element's URL is used.
- `<item><itunes:image>`
  - The URL of the image custom field specified at episode field mapping is used.
- `<item><itunes:explicit>`
  - The value of the epsiodeExplicit native field is used.
- `<item><itunes:episode>`
  - The value of the epsiodeNumber native field is used.
- `<item><itunes:season>`
  - The value of the episodeSeason native field is used.
- `<item><itunes:episodeType>`
  - The value of the episodeType native field is used.
- `<item><itunes:block>`
  - The value of the episodeBlock native field is used.
- `<item><itunes:keywords>`
  - The values of custom field specified at episode field mapping is used.

Additionally, there is a podcast native field podcastRedirectTo. if this field has a value, the podcast RSS page redirects to the value of this field.

## GraphQL:

### Example for fetching podcasts:

```
{
podcasts(podcastFormat: "podcastFormat1") // or instead of podcasts use podcast for fetching only one podcast
  {
    title,
    podcastType, // native field
    ... on podcastFormat1_Podcast{ // use podcastFormatHandle_Podcast to get custom fields
      podcastDescription, // custom field available on podcast format with handle of podcastFormat1
      podcastImage{
        url
      },
      podcastCategory{
        title
      }
    },
  }
}
```

### Example for fetching episodes:

```
{
  episodes // or episode for fetching only one episode
  {
    title,
    episodeExplicit, // native field
    duration @SecToTime, // get duration native field in seconds format, optionally use @SecToTime directive to convert duration to HH:MM:SS format
    ... on podcastFormat1_Episode{ // use podcastFormatHandle_Episode to get custom fields
      episodeDescription,
      episodeAsset{
        url
      }
    }
  }
}
```
### GraphQL permissions:
- View all podcastFormats: a schema with this access can query podcasts and episodes for all podcasts
- View podcast format - [podcast format name]: a schema with this access can query podcasts and episodes only related to this podcast format.
  - Important notes: because schema access is based on podcast format, if you want to separate schema access by each podcast, you should use a different podcast format for each podcast.

## Console Commands
There is a tool available via command line interface (CLI) that runs in a terminal.

### Resaving podcasts
For resaving podcast elements, use `php craft studio/resave/podcasts`.

### Resaving episodes
For resaving episode elements, use `php craft studio/resave/episodes`.
Available options for resaving episodes are:
Option | Description
--- | ---
*--metadata* | Try to fetch metadata (except image metadata) from specified audio/video custom field and set fetched metadata to related fields.
*--imageMetadata* | Try to fetch image metadata from the specified audio/video custom field and set fetched metadata to related fields.
*--previewMetadata* | Only show fetched metadata without saving anything to the episode. Preview is shown only if --metadata and/or --imageMetadata are specified. 
*--overwriteDuration* | Without using this option, if the duration native field has value, it is not overwritten.
*--overwriteGenre* | Without using this option, if the custom field specified for importing genre metadata, has value, it is not overwritten.
*--overwriteImage* | Without using this option, if the custom image field has value, it is not overwritten.
*--overwriteNumber* | Without using this option, if the epsiodeNumber field has value, it is not overwritten.
*--overwritePubDate* | Without using this option, if the pubDate field has value, it is not overwritten.
*--overwriteTitle* | Without using this option, if the title field has value, it is not overwritten.
*--allowEmptyMetaValue* | Use this option to prevent overwriting by null values.

## Templating:
### Getting Podcasts:
You can get podcasts from your templates with craft.podcasts
```
{% set podcasts = craft.podcasts.all() %}
{% for podcast in podcasts %}
  {{ podcast.title }}
{% endfor %}
```

You can get podcasts from your plugin with Podcast::find()
```
use vnali\studio\elements\podcast as PodcastElement;
$podcasts = PodcastElement::find()->all();
```

### Getting Episodes:
You can get episodes from your templates with craft.episodes
```
{% set episodes = craft.episodes.all() %}
{% for episode in episodes %}
  {{ episode.title }}
{% endfor %}
```

You can get episodes from your plugin with Episode::find()
```
use vnali\studio\elements\episode as EpisodeElement;
$episodes = EpisodeElement::find()->podcastId($id)->all();
```

## Permissions

Label | Permission | Description
--- | --- | ---
Manage Podcasts | *studio-managePodcasts* | Can view the podcast element index page and create a podcast element.
Manage Episodes | *studio-manageEpisodes* | Can view the episode element index page and create an episode element (for every podcast).
Create a draft for new podcasts | *studio-createDraftNewPodcasts* | Can view create/view/resave/delete a draft new podcast.
Import Category | *studio-importCategory* | Can import categories to section/category groups.
Manage plugin Settings | *studio-manageSettings* | Managing plugin settings.
View the podcast | *studio-viewPodcast-[PodcastUID]* | Can view a specified podcast and drafts for the podacst created by the user.
Create drafts for the podcast | *studio-createDraftPodcast-[PodcastUID]* | Can create a draft for the specified podcast.
Edit the podcast | *studio-editPodcast-[PodcastUID]* | Can edit a specified a podcast.
Delete own drafts for the podcast | *studio-deleteDraftPodcast-[PodcastUID]* | Can delete an own podacst draft for the specified podcast.
View other user drafts for the podcast | *studio-viewOtherUserDraftPodcast-[PodcastUID]* | Can view other user drafts for the podcast.
Save other user drafts for the podcast | *studio-saveOtherUserDraftPodcast-[PodcastUID]* | Can save other user drafts for the podcast.
Delete other user drafts for the podcast | *studio-deleteOtherUserDraftPodcast-[PodcastUID]* | Can delete other user drafts for the podcast.
Delete the podcast | *studio-deletePodcast-[PodcastUID]* | Can delete the specified podcast.
Set general settings for the podcast | *studio-editPodcastGeneralSettings-[PodcastUID]* | Set general settings for specified podcast.
Set episode settings for the podcast | *studio-editPodcastEpisodeSettings-[PodcastUID]* | Set episode settings for specified podcast.
View episodes | *studio-viewPodcastEpisodes-[PodcastUID]* | Can view episodes of a specified podcast and their drafts created by the user.
Create draft episodes | *studio-createDraftEpisodes-[PodcastUID]* | Create a draft episode for a specified podcast.
Create episodes | *studio-createEpisodes-[PodcastUID]* | Create an episode for a specified podcast.
Delete own drafts for episodes | *studio-deleteDraftEpisodes-[PodcastUID]* | Can delete an own epsiode draft for the specified podcast.
Delete own episodes | *studio-deleteEpisodes-[PodcastUID]* | Delete own episodes for specified podcast.
View other user drafts for episodes | *studio-viewOtherUserDraftEpisodes-[PodcastUID]* | Can view other user drafts for episodes of the specified podcast.
Save other user drafts for episodes | *studio-saveOtherUserDraftEpisodes-[PodcastUID]* | Can save other user drafts for episodes of the specified podcast.
Delete other user drafts for episodes | *studio-deleteOtherUserDraftEpisodes-[PodcastUID]* | Can delete other user drafts for episodes of the specified podcast.
View other user episodes | *studio-viewOtherUserEpisodes-[PodcastUID]* | Can view other user episodes of the specified podcast.
Save other user episodes | *studio-saveOtherUserEpisodes-[PodcastUID]* | Can save other user episodes of the specified podcast.
Delete other user episodes | *studio-deleteOtherUserEpisodes-[PodcastUID]* | Can delete other user episodes of the specified podcast.
Import episodes by URL | *studio-importEpisodeByRSS-[PodcastUID]* | Import episodes from RSS.
Import episodes by Asset index | *studio-importEpisodeByAssetIndex-[PodcastUID]* | Set settings for importing episodes for specified podcast.
View not published RSS | *studio-viewPublishedRSS-[PodcastUID]* | View published RSS for specified podcast -Skip this permission check by enabling 'Allow All to see published RSS' for that podcast-.
View published RSS | *studio-viewNotPublishedRSS-[PodcastUID]* | View not published RSS for specified podcast.

## Other Features

- Providing a preview option for audio assets
- Easy search and filter for podcasts and episodes by various conditions for native fields.
  - Available Podcast conditions: author name, owner name, owner email, copyright, podcats being complete/explicit/complete/new.
  - Available Episode conditions: duration, season, type, number, and the episode being explicit/blocked.
- You can import [sample podcast categories](https://podcasters.apple.com/support/1691-apple-podcasts-categories), quickly via (Import->Import Category). these samples can be imported only to empty sections/category groups.

## FAQ

> There is no New podcast/episode button on the element index page.  

To be able to create a podcast/an episode, the site should be enabled for that item in general settings (settings->podcast formats->general).  

> Episodes are not created on asset indexing   

Make sure indexed volume is added to for intended podcast on import episodes from the Asset index page.  

## Contact
Feel free to contact me by email at vnali.dev@gmail.com or direct message me via 'vnali' on [Craft CMS Discord](https://craftcms.com/discord) channel.
