# plugin-favicons
![Favicon](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favicon.jpeg)
A plugin for [Micro.blog](https://micro.blog "Micro.blog") that injects favicon meta into the page `<head>`.

## Generating Resources

Let’s start with how to acquire all the favicon images we want to link. Start with a square image of your choice at least 300-some pixels in both directions.

If you run that image through [https://realfavicongenerator.net](https://realfavicongenerator.net) you’ll get a nice little kit to download and unzip that will contain a bunch of resources. The same thing can be said of [https://favicomatic.com](https://favicomatic.com). Combine the two and you get sh$t tons of favicon files.

### Using RealFaviconGenerator

Click the blue button and give it an image.
![Generator](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favicon_generator.png)

Configure your options. 
![Options 1](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_1.png)

![Options 2](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_2.png)

![Options 3](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_3.png)

Then download the package by clicking the blue button.
![Package](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favicon_package.png)

The chunk of code on display contains the injectable bits you got from using *RealFaviconGenerator*:

```html

<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">
```

Unzip your favicon package, you get something kinda like:

![RealFaviconGenerator Package](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/realfavicongenerator-package.jpeg)


### Using Favic-O-Matic

If you want it all, start by selecting `Every damn size, sir!`.

![Favic-O-Matic](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favic-o-matic.jpeg)

If you really want it all, click `advanced settings` and starting ticking boxes.
![Favic-O-Matic Advanced](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favic-o-matic-advanced.jpeg)

Download and unzip … you oughta have something kinda like:

![Favic-O-Matic Package](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favic-o-matic-package.jpeg)

The chunk of code in `code.txt` contains the injectable bits you got from the *Favic-O-Matic* generator:

```html
<link rel="apple-touch-icon-precomposed" sizes="57x57" href="https://moondeer.blog/apple-touch-icon-57x57.png" />
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="https://moondeer.blog/apple-touch-icon-114x114.png" />
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="https://moondeer.blog/apple-touch-icon-72x72.png" />
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="https://moondeer.blog/apple-touch-icon-144x144.png" />
<link rel="apple-touch-icon-precomposed" sizes="60x60" href="https://moondeer.blog/apple-touch-icon-60x60.png" />
<link rel="apple-touch-icon-precomposed" sizes="120x120" href="https://moondeer.blog/apple-touch-icon-120x120.png" />
<link rel="apple-touch-icon-precomposed" sizes="76x76" href="https://moondeer.blog/apple-touch-icon-76x76.png" />
<link rel="apple-touch-icon-precomposed" sizes="152x152" href="https://moondeer.blog/apple-touch-icon-152x152.png" />
<link rel="icon" type="image/png" href="https://moondeer.blog/favicon-196x196.png" sizes="196x196" />
<link rel="icon" type="image/png" href="https://moondeer.blog/favicon-96x96.png" sizes="96x96" />
<link rel="icon" type="image/png" href="https://moondeer.blog/favicon-32x32.png" sizes="32x32" />
<link rel="icon" type="image/png" href="https://moondeer.blog/favicon-16x16.png" sizes="16x16" />
<link rel="icon" type="image/png" href="https://moondeer.blog/favicon-128.png" sizes="128x128" />
<meta name="application-name" content="Moondeer"/>
<meta name="msapplication-TileColor" content="#FFFFFF" />
<meta name="msapplication-TileImage" content="https://moondeer.blog/mstile-144x144.png" />
<meta name="msapplication-square70x70logo" content="https://moondeer.blog/mstile-70x70.png" />
<meta name="msapplication-square150x150logo" content="https://moondeer.blog/mstile-150x150.png" />
<meta name="msapplication-wide310x150logo" content="https://moondeer.blog/mstile-310x150.png" />
<meta name="msapplication-square310x310logo" content="https://moondeer.blog/mstile-310x310.png" />
<meta name="msapplication-notification" content="frequency=30;polling-uri=https://notifications.buildmypinnedsite.com/?feed=https://moondeer.blog/feed.xml&amp;id=1;polling-uri2=https://notifications.buildmypinnedsite.com/?feed=https://moondeer.blog/feed.xml&amp;id=2;polling-uri3=https://notifications.buildmypinnedsite.com/?feed=https://moondeer.blog/feed.xml&amp;id=3;polling-uri4=https://notifications.buildmypinnedsite.com/?feed=https://moondeer.blog/feed.xml&amp;id=4;polling-uri5=https://notifications.buildmypinnedsite.com/?feed=https://moondeer.blog/feed.xml&amp;id=5;cycle=1" />
```

## Refining Resources
The first thing you get to do is throw any `.ico` files in the trash. The currently is now way that I’ve found to get them onto the server. Should you manage the impossible, the plugin does have parameters you can set to let it know.

Side note: I also went down a base64 rabbit hole with the `.ico` file. I found no use for it. If you want to piddle anyway, you can convert your `.ico` into base64 [here](https://base64.guru/converter/encode/image/ico "Base64 Guru") and paste the value into a plugin parameter.

Okay, so I documented the sh$t out of the data file template. I recommend using it. It looks like this:

```TOML
# Parameters derived from the use of https://realfavicongenerator.net
# and https://favicomatic.com to generate favicon assets. 

# General theme color for the site (used by Mobile Safari in iOS 15 to paint the background of the status bar).
# ThemeColor = "#FFFFFF"

[Apple]

# Path to the 180x180 pixel image.
#
# TouchIcon = "apple-touch-icon.png"

# Path to the 57x57 pixel image.
#
# x57 = "/apple-touch-icon-57x57.png"

# Path to the 114x114 pixel image.
#
# x114 = "/apple-touch-icon-114x114.png"

# Path to the 72x72 pixel image.
#
# x72 = "/apple-touch-icon-72x72.png"

# Path to the 144x144 pixel image.
#
# x144 = "/apple-touch-icon-144x144.png"

# Path to the 60x60 pixel image.
#
# x60 = "/apple-touch-icon-60x60.png"

# Path to the 120x120 pixel image.
#
# x120 = "/apple-touch-icon-120x120.png"

# Path to the 76x76 pixel image.
#
# x76 = "/apple-touch-icon-76x76.png"

# Path to the 152x152 pixel image.
#
# x152 = "/apple-touch-icon-152x152.png"

# Path to the Safari pinned tab SVG file.
# You can create a template file and paste in your SVG or include the file 
# in a custom theme GitHub repository. For it to be at the root, the location would
# be static/safari-pinned-tab.svg
#
# SafariPinnedTab = "/safari-pinned-tab.svg"

[Favicon]

# Path to an SVG favicon image.
#
# svg = "/favicon.svg"

# Path to the 196x196 pixel image.
#
# x196 = "/favicon-196x196.png"

# Path to the 128x128 pixel image.
#
# x128 = "/favicon-128x128.png"

# Path to the 96x96 pixel image.
#
# x96 = "/favicon-96x96.png"

# Path to the 32x32 pixel image.
#
# x32 = "favicon-32x32.png"

# Path to the 16x16 pixel image.
#
# x16 = "favicon-16x16.png"

# Path to regular image. If you upload an ico image it gets converted to jpg
#
# ico = "favicon.ico"

# Base64 ico image.
# You can convert .ico to base64 here: https://base64.guru/converter/encode/image/ico
#
# icoBase64 = "base64"

[AndroidChrome]

# Short name for app-like button.
# This goes in the webmanifest but you can store the value here for reference 
#
# Name = "Short Name"

# Path to the 192x192 pixel image. 
# This goes in the webmanifest but you can store the value here for reference 
#
# x192 = "android-chrome-192x192.png"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
#
# x512 = "android-chrome-512x512.png"

# Theme color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#
# ThemeColor = "#FFFFFF"

# Background color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#
# BackgroundColor = "#FFFFFF"

# In this plugin or your custom theme, create template 'static/site.webmanifest'
# If you're using GitHub with your custom them, add a `.txt` extension so the file
# may be cloned from your repository.
#
# Replace bracketed variable names with their values.
#
# {
#     "name": "[AndroidChrome.Name]",
#     "short_name": "[AndroidChrome.Name]",
#     "icons": [
#         {
#             "src": "[AndroidChrome.x192]",
#             "sizes": "192x192",
#             "type": "image/png"
#         },
#         {
#             "src": "[AndroidChrome.x512]",
#             "sizes": "512x512",
#             "type": "image/png"
#         }
#     ],
#     "theme_color": "[AndroidChrome.ThemeColor]",
#     "background_color": "[AndroidChrome.BackgroundColor]",
#     "display": "standalone"
# }
#
# Once the file is in place, uncomment the appropriate entry below.
#
# Webmanifest = "/site.webmanifest"
# Webmanifest = "/site.webmanifest.txt"


[MSTile]

# Short name for app-like button.
#
# ApplicationName = "Short Name"

# Path to the 70x70 pixel image.
#
# x70 = "/mstile-70x70.png"

# Path to the 144x144 pixel image.
#
# x144 = "/mstile-144x144.png"

# Path to the 150x150 pixel image.
#
# x150 = "/mstile-150x150.png"

# Path to the 310x150 pixel image.
#
# Widex150 = "/mstile-310x150.png"

# Path to the 310x310 pixel image.
#
# x310 = "/mstile-310x310.png"

# The tile color behind the 150x150 pixel image.
#
# TileColor = "#DA532C"

# All of the above (with the exception of ApplicationName) can have 
# their meta tags replaced with a single link to an XML file. For this
# to happen, here is what you must to:
#
# In this plugin or your custom theme, create template 'static/browserconfig.xml'
#
# Replace bracketed variable names with their values.
#
# <?xml version="1.0" encoding="utf-8"?>
# <browserconfig>
#   <msapplication>
#     <tile>
#       <square70x70logo src="[MSTile.x70]"/>
#       <square144x144logo src="[MSTile.x144]"/>
#       <square150x150logo src="[MSTile.x150]"/>
#       <wide310x150logo src="[MSTile.Widex150]"/>
#       <square310x310logo src="[MSTile.x310]"/>
#       <TileColor>[MSTile.TileColor]</TileColor>
#     </tile>
#   </msapplication>
# </browserconfig>
# Once the file is in place, uncomment the entry below.
#
# BrowserConfig = "/browserconfig.xml"

```

Go to the `Design` tab, click `Edit Custom Themes`, click this plugin, and find the above file (`data/plugin_favicons/params.toml`).

### File Location for PNG Files

#### Option One (You are using a custom theme with a GitHub repository)

You can stick everything directly under `static`, push, clone … you know the drill.

![Static Directory](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/static_directory.jpeg)

Afterwards, match up the PNG files with the commented lines of code and uncomment them.

#### Option Two (You must or would rather upload each file)

For each PNG image, follow these steps.
1. Upload the image
2. Copy the address of the newly uploaded image
3. Find the matching commented line of code for the image
4. Paste in the actual location and uncomment the now valid variable value

### File Location for SVG Files

These are easier, you just can’t upload them. You can clone them if you’re using a custom theme with a GitHub repository; or, you can copy/paste their text content into new template files. The two supported SVG FIles are `Favicon.svg` and `Apple.SafariPinnedTab`.

As before, find the line for the corresponding variable and uncomment it once you have a valid path declared.


### File Location for the XML File

Instead of including a bunch of Microsoft meta tags, a single XML file can be used to store all the `MSTile` stuff but `MSTile.ApplicationName`.

To do this, create an XML file called `browserconfig.xml`, copy the template into it, and then replace all the bracketed variables with the values you entered into `data/plugin_favicons/params.toml`. You can remove lines for which you have no matching image.

```xml
<?xml version="1.0" encoding="utf-8"?>
<browserconfig>
  <msapplication>
    <tile>
      <square70x70logo src="[MSTile.x70]"/>
      <square144x144logo src="[MSTile.x144]"/>
      <square150x150logo src="[MSTile.x150]"/>
      <wide310x150logo src="[MSTile.Widex150]"/>
      <square310x310logo src="[MSTile.x310]"/>
      <TileColor>[MSTile.TileColor]</TileColor>
    </tile>
  </msapplication>
</browserconfig>
```

Once you’ve done that, find the `MSTile.BrowserConfig` variable, enter the path to your file (minus the `static`), and uncomment the now valid variable.

### File Location for the Webmanifest

The last bit of nonsense is for appeasing Google. We need a simply JSON file. 

#### Option One (You are using a custom theme with a GitHub repository)

[Micro.blog](https://micro.blog "Micro.blog") will not clone a file called `site.webmanifest`. Call that sh$t `site.webmanifest.txt`.

#### Option Two (You want to recreate this file every time you reinstall)

Go ahead and call that file whatever TF you feel like calling it.


#### Moving On

With your new file created, copy this template and paste it in thar:

```json
{
    "name": "[AndroidChrome.Name]",
    "short_name": "[AndroidChrome.Name]",
    "icons": [
        {
            "src": "[AndroidChrome.x192]",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "[AndroidChrome.x512]",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "theme_color": "[AndroidChrome.ThemeColor]",
    "background_color": "[AndroidChrome.BackgroundColor]",
    "display": "standalone"
}
```

Replace the bracketed variables with the values you entered into `data/plugin_favicons/params.toml`.

When you’ve done that, go back the data file, find the commented line of code for `AndroidChrome.Webmanifest`, enter the path to your new file, and then uncomment the now totally valid variable value.

## Persistence

Once you’re happy with your file, consider editing your custom theme (or creating a new custom theme from the Micro.blog theme you are using so you can add a file to it) by creating a new template file at `data/plugin_favicons_params.toml` and pasting in everything from the plugin file `data/plugin_favicons/params.toml`. The new file will take precedence and it won’t get deleted when updating the plugin.

## Confirmation

You can head back to [https://realfavicongenerator.net](https://realfavicongenerator.net) and check your site. When everything is properly in place, you get mostly green results like those below. When everything is properly in place, you should get mostly green … give or take some quibbles regarding whether or not an image is in the root directory.

If anyone is curious, my data file lives at `data/plugin_favicons_params.toml` inside [theme-moondeer](https://github.com/moonbuck/theme-moondeer "theme-moondeer"), and it looks like this:

```TOML
# Parameters derived from the use of https://realfavicongenerator.net
# and https://favicomatic.com to generate favicon assets. 

# General theme color for the site (used by Mobile Safari in iOS 15 to paint the background of the status bar).
ThemeColor = "#516189"

[Apple]

# Path to the 180x180 pixel image.
#
TouchIcon = "apple-touch-icon.png"

# Path to the 57x57 pixel image.
#
x57 = "/apple-touch-icon-57x57.png"

# Path to the 114x114 pixel image.
#
x114 = "/apple-touch-icon-114x114.png"

# Path to the 72x72 pixel image.
#
x72 = "/apple-touch-icon-72x72.png"

# Path to the 144x144 pixel image.
#
x144 = "/apple-touch-icon-144x144.png"

# Path to the 60x60 pixel image.
#
x60 = "/apple-touch-icon-60x60.png"

# Path to the 120x120 pixel image.
#
x120 = "/apple-touch-icon-120x120.png"

# Path to the 76x76 pixel image.
#
x76 = "/apple-touch-icon-76x76.png"

# Path to the 152x152 pixel image.
#
x152 = "/apple-touch-icon-152x152.png"

# Path to the Safari pinned tab SVG file.
# You can create a template file and paste in your SVG or include the file 
# in a custom theme GitHub repository. For it to be at the root, the location would
# be static/safari-pinned-tab.svg
#
SafariPinnedTab = "/safari-pinned-tab.svg"

[Favicon]

# Path to an SVG favicon image.
#
svg = "/favicon.svg"

# Path to the 196x196 pixel image.
#
x196 = "/favicon-196x196.png"

# Path to the 128x128 pixel image.
#
x128 = "/favicon-128x128.png"

# Path to the 96x96 pixel image.
#
x96 = "/favicon-96x96.png"

# Path to the 32x32 pixel image.
#
x32 = "favicon-32x32.png"

# Path to the 16x16 pixel image.
#
x16 = "favicon-16x16.png"

# Path to regular image. If you upload an ico image it gets converted to jpg
#
# ico = "favicon.ico"

# Base64 ico image.
# You can convert .ico to base64 here: https://base64.guru/converter/encode/image/ico
#
# icoBase64 = "base64"

[AndroidChrome]

# Short name for app-like button.
# This goes in the webmanifest but you can store the value here for reference 
#
Name = "Moondeer"

# Path to the 192x192 pixel image. 
# This goes in the webmanifest but you can store the value here for reference 
#
x192 = "android-chrome-192x192.png"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
#
x512 = "android-chrome-512x512.png"

# Theme color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#
ThemeColor = "#516189"

# Background color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#
BackgroundColor = "#516189"

# In this plugin or your custom theme, create template 'static/site.webmanifest'
# If you're using GitHub with your custom them, add a `.txt` extension so the file
# may be cloned from your repository.
#
# Replace bracketed variable names with their values.
#
# {
#     "name": "[AndroidChrome.Name]",
#     "short_name": "[AndroidChrome.Name]",
#     "icons": [
#         {
#             "src": "[AndroidChrome.x192]",
#             "sizes": "192x192",
#             "type": "image/png"
#         },
#         {
#             "src": "[AndroidChrome.x512]",
#             "sizes": "512x512",
#             "type": "image/png"
#         }
#     ],
#     "theme_color": "[AndroidChrome.ThemeColor]",
#     "background_color": "[AndroidChrome.BackgroundColor]",
#     "display": "standalone"
# }
#
# Once the file is in place, uncomment the appropriate entry below.
#
# Webmanifest = "/site.webmanifest"
Webmanifest = "/site.webmanifest.txt"


[MSTile]

# Short name for app-like button.
#
ApplicationName = "Moondeer"

# Path to the 70x70 pixel image.
#
x70 = "/mstile-70x70.png"

# Path to the 144x144 pixel image.
#
x144 = "/mstile-144x144.png"

# Path to the 150x150 pixel image.
#
x150 = "/mstile-150x150.png"

# Path to the 310x150 pixel image.
#
Widex150 = "/mstile-310x150.png"

# Path to the 310x310 pixel image.
#
x310 = "/mstile-310x310.png"

# The tile color behind the 150x150 pixel image.
#
TileColor = "#DA532C"

# All of the above (with the exception of ApplicationName) can have 
# their meta tags replaced with a single link to an XML file. For this
# to happen, here is what you must to:
#
# In this plugin or your custom theme, create template 'static/browserconfig.xml'
#
# Replace bracketed variable names with their values.
#
# <?xml version="1.0" encoding="utf-8"?>
# <browserconfig>
#   <msapplication>
#     <tile>
#       <square70x70logo src="[MSTile.x70]"/>
#       <square144x144logo src="[MSTile.x144]"/>
#       <square150x150logo src="[MSTile.x150]"/>
#       <wide310x150logo src="[MSTile.Widex150]"/>
#       <square310x310logo src="[MSTile.x310]"/>
#       <TileColor>[MSTile.TileColor]</TileColor>
#     </tile>
#   </msapplication>
# </browserconfig>
# Once the file is in place, uncomment the entry below.
#
BrowserConfig = "/browserconfig.xml"

```
