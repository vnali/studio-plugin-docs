## Introduction
This is a [Craft CMS](https://craftcms.com/) plugin for managing podcasts and episodes.
This plugin is in the development phase so API can be changed.

## Requirement
- Craft 4.3 and higher.

## Main features:
- Manage multiple podcasts.
- Multi-site support for the podcast and episode elements.
- Each podcast can have a different podcast format, settings, and field layout.
- Bulk import episodes via fetching from RSS and Asset index utility.
- Fetch Id3 metadata (title, duration, track number, images) from the episode track and save it to custom/native fields
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
  - copyright
  - podcastRedirectTo: if you decided to redirect feed RSS to a new feed, you can set this native field.  
  - podcastIsNewFeedUrl: if this is a new feed URL, and an old feed is redirected to this feed.
  
  You can create custom fields and add them to the field layout via (Plugin settings->Podcast Formats->Podcast field layout).  
  Optionally, for testing purposes, when dev mode is on, you can import sample custom fields quickly via (Plugin settings->Import podcast fields).  
  After creating custom fields, make sure to check the podcast mapping page (Plugin settings->Podcast Formats->Podcast mapping), to specify which custom field is used for the podcast image, podcast description, or podcast category.  
   
<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/221909217-405423b3-0288-4f19-a731-e8edc98555a2.jpg" width="800px">  
</p>  

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
After creating custom fields, make sure to check the episode mapping page (Plugin settings->Podcast Formats->Episode mapping), to specify which custom field is used for the episode file, episode image, episode description, and episode publish date.

### Create episodes manually
- Go to the episodes index page.
- Select the podcast you want to create an episode in.
- Add an asset as an episode file.
  - Enable fetch Id3 metadata/fetch Id3 image metadata and click on the fetch button to get title/duration/image/year/genre metadata from the episode file and fill automatically related native/custom fields.
  - Important note: you should specify that fetched metadata is written to which custom field via the episode mapping page (Plugin settings->Podcast Formats->Episode mapping)
- Finally, review and save the episode.

### Importing episode via Asset Indexes utility
- Go to the podcast index page and click on the desired podcast and from the action list select 'Import episodes by asset index'.
- In "Import episode when indexing assets of these volumes" field, select the volumes you uploaded episodes.
- Make sure "enable import episodes" is enabled.
- Save this form and use the Asset index utility tool to index assets on those volumes.
- Check if assets are imported to Craft and an episode is created for each asset file.
  - Ensure that you specified the episode asset field in the episode mapping page. Otherwise, the plugin can't create an episode from the asset.
- Finally, disable "enable import episodes" to prevent accidental import.

### Importing episodes via fetching data from the RSS
- Go to the podcast index page and click on the desired podcast and from the action list select 'Import episodes by URL'.
- In the "Import episode from RSS" field, specify the URL you would like to fetch episodes from it.
- If you would like to limit imported episodes, enter a value for the limit field.
- There is an ignoreMainAsset option to prevent fetching large main asset via CURL
- Save this form.
- The plugin tries to fetch episode contents from that URL and create episodes via a Craft job.

<p align="center">
<img src="https://user-images.githubusercontent.com/55586085/221927757-873abe7e-aea2-4806-8db3-ac3525f3b55a.jpg" width="800px">  
</p>

## Podcast RSS
On the podcast element index page, for a live podcast, we can see an RSS link.  
For disabled podcasts, there is a preview RSS link that only users with access to that podcast can see it.  
Below you can see how `<channel>` and `<item>` tags are created:
- `<channel><title>/<channel><itunes:title>`
  - The podcast title is used.
- `<channel><description>`
  - The value of the text field specified on the podcast field mapping page is used.
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
- `<item><title>/<item><itunes:title>`
  - The episode title is used.
- `<item><enclosure>`
  - The URL, type, and length of the asset for the specified episode's asset field are used.
- `<item><guid>`
  - The value of the GUID native field is used.
- `<item><pubDate>`
  - The value of the specified date custom field is used. if there is no mapping, episode creates date is used.
- `<item><description>`
  - The value of the text custom field for description specified in podcast field mapping is used.
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
- `<item><itunes:itunes:block>`
  - The value of the episodeBlock native field is used.

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
*--metadata* | Try to fetch metadata (except image metadata) from specified audio/video custom field and set fetched metadata to related fields
*--imageMetadata* | Try to fetch image metadata from the specified audio/video custom field and set fetched metadata to related fields
*--previewMetadata* | Only show fetched metadata without saving anything to custom fields
*--overwriteImage* | Without using this option, if the custom image field has value, it is not overwritten.
*--overwriteNumber* | Without using this option, if the epsiodeNumber field has value, it is not overwritten.
*--overwriteTitle* | Without using this option, if the title field has value, it is not overwritten.
*--ifMetaValueNotEmpty* | Use this option to prevent overwriting by null values

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

Permission | Description
--- | ---
*studio-managePodcasts* | Can view the podcast element index page and create a podcast element.
*studio-manageEpisodes* | Can view the episode element index page and create an episode element (for every podcast).
*studio-createDraftNewPodcasts* | Can view create/view/resave/delete a draft new podcast.
*studio-viewPodcast-[PodcastUID]* | Can view a specified podcast and drafts for the podacst created by user.
*studio-createDraftPodcast-[PodcastUID]* | Can create a draft for the specified podcast.
*studio-editPodcast-[PodcastUID]* | Can edit a specified a podcast.
*studio-deleteDraftPodcast-[PodcastUID]* | Can delete an own podacst draft for the specified podcast.
*studio-viewOtherUserDraftPodcast-[PodcastUID]* | Can view other user drafts for the podcast.
*studio-saveOtherUserDraftPodcast-[PodcastUID]* | Can save other user drafts for the podcast.
*studio-deleteOtherUserDraftPodcast-[PodcastUID]* | Can delete other user drafts for the podcast.
*studio-deletePodcast-[PodcastUID]* | Can delete a specified podcast.
*studio-editPodcastGeneralSettings-[PodcastUID]* | Set general settings for specified podcast.
*studio-editPodcastEpisodeSettings-[PodcastUID]* | Set episode settings for specified podcast.
*studio-viewPodcastEpisodes-[PodcastUID]* | Can view episodes of a specified podcast and their drafts created by the user.
*studio-createDraftEpisodes-[PodcastUID]* | Create a draft episode for a specified podcast.
*studio-deleteDraftEpisodes-[PodcastUID]* | Can delete an own epsiode draft for the specified podcast.
*studio-createEpisodes-[PodcastUID]* | Create an episode for a specified podcast.
*studio-viewOtherUserDraftEpisodes-[PodcastUID]* | Can view other user drafts for episodes of the specified podcast.
*studio-saveOtherUserDraftEpisodes-[PodcastUID]* | Can save other user drafts for episodes of the specified podcast.
*studio-deleteOtherUserDraftEpisodes-[PodcastUID]* | Can delete other user drafts for episodes of the specified podcast.
*studio-deleteEpisodes-[PodcastUID]* | Delete episodes for specified podcast.
*studio-importEpisodeByRSS-[PodcastUID]* | Import episodes from RSS.
*studio-importEpisodeByAssetIndex-[PodcastUID]* | Set settings for importing episodes for specified podcast.
*studio-viewPublishedRSS-[PodcastUID]* | View published RSS for specified podcast -Skip this permission check by enabling $allowAllToSeeRSS podcast setting-.
*studio-importCategory* | Can import categories to section/category groups.
*studio-manageSettings* | Managing plugin settings.

## Other Features

- Providing a preview option for audio assets
- Easy search and filter for podcasts and episodes by various conditions for native fields.
  - Available Podcast conditions: author name, owner name, owner email, copyright, podcats being complete/explicit/complete/new.
  - Available Episode conditions: duration, season, type, number, and the episode being explicit/blocked.
- You can import sample podcast categories quickly via (Import->Import Category). these samples can be imported only to empty sections/category groups.

## FAQ

> There is no New podcast/episode button on the element index page.  

To be able to create a podcast/an episode, the site should be enabled for that item in general settings (settings->podcast formats->general).  

> Episodes are not created on asset indexing   

Make sure indexed volume is added to for intended podcast on import episodes from the Asset index page.  

## License & Pricing

This is a commercial plugin available through the Craft plugin store.

## Contact
Feel free to contact me by email at vnali.dev@gmail.com or direct message me via 'vnali' on [Craft CMS Discord](https://craftcms.com/discord) channel.
