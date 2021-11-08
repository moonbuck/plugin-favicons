# plugin-favicons
A plugin for [Micro.blog](https://micro.blog "Micro.blog") that injects favicon meta into the page `<head>`. I used [https://realfavicongenerator.net](https://realfavicongenerator.net) to generate my favicons so this plugin basically gets the generated favicon package into your Hugo theme the best it can.

There is only one partial. It lives at `layouts/partials/favicons.html`
and looks like this:

```go
{{- $theme := site.Data.plugin_favicons_params }}
{{- $plugin := site.Data.plugin_favicons.params }}
{{- $theme_color := ($theme.ThemeColor | default $plugin.ThemeColor) | default "#FFFFFF" }}
{{- $touch_icon := $theme.Apple.TouchIcon | default $plugin.Apple.TouchIcon }}
{{- $safari_pinned_tab := $theme.Apple.SafariPinnedTab | default $plugin.Apple.SafariPinnedTab }}
{{- $favicon_32 := $theme.Favicon.x32 | default $plugin.Favicon.x32 }}
{{- $favicon_16 := $theme.Favicon.x16 | default $plugin.Favicon.x16 }}
{{- $favicon_ico_base64 := $theme.Favicon.icoBase64 | default $plugin.Favicon.icoBase64 }}
{{- $favicon_ico := $theme.Favicon.ico | default $plugin.Favicon.ico }}
{{- $manifest := $theme.AndroidChrome.Webmanifest | default $plugin.AndroidChrome.Webmanifest }}
{{- $browser_config := $theme.MSTile.BrowserConfig | default $plugin.MSTile.BrowserConfig }}
{{- $mstile_color := $theme.MSTile.TileColor | default $plugin.MSTile.TileColor }}


{{- with $touch_icon }}<link rel="apple-touch-icon" sizes="180x180" href="{{ . }}" />{{ end }}
{{ with $favicon_32 }}<link rel="icon" type="image/png" sizes="32x32" href="{{ . }}" />{{ end }}
{{ with $favicon_16 }}<link rel="icon" type="image/png" sizes="16x16" href="{{ . }}" />{{ end }}
{{ with $favicon_ico_base64 }}<link rel="shortcut icon" href="data:image/x-icon;base64,{{ . }}" />{{ else }}
{{ with $favicon_ico }}<link rel="shortcut icon" href="{{ . }}" />{{ end }}{{ end }}
{{ with $manifest }}<link rel="manifest" href="{{ . }}" />{{ end }}
{{ with $safari_pinned_tab }}<link rel="mask-icon" href="{{ . }}" color="{{ $theme_color }}" />{{ end }}
{{ with $mstile_color }}<meta name="msapplication-TileColor" content="{{ . }}">{{ end }}
{{ with $browser_config }}<meta name="msapplication-config" content="{{ . }}">{{ end }}
{{ with $theme_color }}<meta name="theme-color" content="{{ . }}">{{ end }}
```

The parameters are stored in a TOML file living at `data/plugin_favicons/params.toml` and it starts out like this:

```TOML
# Parameters derived from the use of https://realfavicongenerator.net
# to generate favicon assets. 

# General theme color for the site (used by Mobile Safari in iOS 15 to paint the background of the status bar).
#ThemeColor = "#FFFFFF"

[Apple]

# Path to the 180x180 pixel image.
#TouchIcon = "apple-touch-icon.png"

# Path to the Safari pinned tab SVG file.
# Manually copy/paste your SVG content into static/assets/favicons/safari-pinned-tab.svg 
# or provide a path to a suitable SVG file … otherwise leave it commented out.
#SafariPinnedTab = "/assets/favicons/safari-pinned-tab.svg"

[Favicon]

# Path to the 32x32 pixel image.
#x32 = "favicon-32x32.png"

# Path to the 16x16 pixel image.
#x16 = "favicon-16x16.png"

# Path to regular image. If you upload an ico image it gets converted to jpg
#ico = "favicon.ico"

# Base64 ico image.
# You can convert .ico to base64 here: https://base64.guru/converter/encode/image/ico
#icoBase64 = "base64"

[AndroidChrome]

# Manually copy/paste your site.webmanifest content into static/assets/favicons/site.webmanifest 
# or provide a path to a suitable mobile app manifest file … otherwise leave it commented out.
#Webmanifest = "/assets/favicons/site.webmanifest"

# Path to the 192x192 pixel image. 
# This goes in the webmanifest but you can store the value here for reference 
#x192 = "android-chrome-192x192.png"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
#x512 = "android-chrome-512x512.png"

# Theme color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#ThemeColor = "#FFFFFF"

# Background color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
#BackgroundColor = "#FFFFFF"

[MSTile]

# Manually copy/paste your browserconfig.xml content into static/assets/favicons/browserconfig.xml 
# or provide a path to a suitable browserconfig file … otherwise leave it commented out.
#BrowserConfig = "/assets/favicons/browserconfig.xml"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
#x512 = "android-chrome-512x512.png"

# The tile color behind the 150x150 pixel image.
#TileColor = "#DA532C"
```

Once you’re happy with your file, you can copy it into your theme at the following location to persists between plugin updates: `data/plugin_favicons_params.toml`.

The parameter values are basically plug and play with what [https://realfavicongenerator.net](https://realfavicongenerator.net) generates for you.

Click the blue button and give it an image.
![Generator](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favicon_generator.png)

Configure your options. 
![Options 1](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_1.png)

![Options 2](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_2.png)

![Options 3](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/generator_options_3.png)

Then download the package by clicking the blue button.
![Package](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/favicon_package.png)

The chunk of HTML above is basically what this plugin injects; but, we have to get your resources in place.

Unzip your favicon package, you get something kinda like:
![Unzipped Package](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/unzipped_package.png)

Let’s start with the easy ones. These are the images you simply upload, grab the address, and then plop into that TOML file. Do this for the following parameters by matching the placeholder value to a file in the package you downloaded: `Apple.TouchIcon`, `Favicon.x32`, `Favicon.x16`, `Favicon.ico`, `AndroidChrome.x192`, `AndroidChrome.x512`, and `MSTile.x150`.

You can also go ahead and fill in all those Hex color values. The `ThemeColor` value at the very top will show up behind the status bar in Mobile Safari in iOS 15.

Now it’s time to get to the text content of that SVG file. However you manage it (I used Textastic), take that code and paste it in place of the existing content in the file that lives at `static/assets/favicons/safari-pinned-tab.svg`.

Next you’ll wanna crack open `static/assets/favicons/browserconfig.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?><browserconfig><msapplication><tile><square150x150logo src="PATH"/><TileColor>HEX</TileColor></tile></msapplication></browserconfig>
```

Take your `MSTile.x150` value and paste it where it currently says `PATH`. Take your `MSTile.TileColor` and paste it where it currently says `HEX`.

The last bit of pasting you’ll want to do is in the webmanifest (which I slapped the `txt` extension on to get it through the selective cloning process) that lives at `static/assets/favicons/site.webmanifest.txt`:

```json
{
    "name": "NAME",
    "short_name": "NAME",
    "icons": [
        {
            "src": "PATH",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "PATH",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "theme_color": "HEX",
    "background_color": "HEX",
    "display": "standalone"
}
```

Pick a short name and slap it where it says `NAME`. Take your `AndroidChrome.x192` value and paste it where it says `PATH` for sizes `192x192`. Take your `AndroidChrome.x512` value and paste it where it says `PATH` for sizes `512x512`. Paste your `AndroidChrome.ThemeColor` and `AndroidChrome.BackgroundColor` Hex color values while you are at it.

Make sure you’ve removed the `#` from the front of all your valid parameter values and you should be totally set. My parameters would look like this:

```TOML
# Parameters derived from the use of https://realfavicongenerator.net
# to generate favicon assets. 

# General theme color for the site (used by Mobile Safari in iOS 15 to paint the background of the status bar).
ThemeColor = "#516189"

[Apple]

# Path to the 180x180 pixel image.
TouchIcon = "https://moondeer.blog/uploads/2021/0ac094eaf0.png"

# Path to the Safari pinned tab SVG file.
# Manually copy/paste your SVG content into static/assets/favicons/safari-pinned-tab.svg 
# or provide a path to a suitable SVG file … otherwise leave it commented out.
SafariPinnedTab = "/assets/favicons/safari-pinned-tab.svg"

[Favicon]

# Path to the 32x32 pixel image.
x32 = "https://moondeer.blog/uploads/2021/6e917ca7cb.png"

# Path to the 16x16 pixel image.
x16 = "https://moondeer.blog/uploads/2021/cde89be874.png"

# Path to regular image. If you upload an ico image it gets converted to jpg
#ico = "https://moondeer.blog/uploads/2021/d7e47b4fbb.jpg"

# Base64 ico image.
# You can convert .ico to base64 here: https://base64.guru/converter/encode/image/ico
#icoBase64 = "base64"

[AndroidChrome]

# Manually copy/paste your site.webmanifest content into static/assets/favicons/site.webmanifest.txt 
# or provide a path to a suitable mobile app manifest file … otherwise leave it commented out.
Webmanifest = "/assets/favicons/site.webmanifest.txt"

# Path to the 192x192 pixel image. 
# This goes in the webmanifest but you can store the value here for reference 
x192 = "https://moondeer.blog/uploads/2021/a893f9ebc9.png"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
x512 = "https://moondeer.blog/uploads/2021/e4cfc0ba68.png"

# Theme color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
ThemeColor = "#516189"

# Background color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
BackgroundColor = "#516189"


[MSTile]

# Manually copy/paste your browserconfig.xml content into static/assets/favicons/browserconfig.xml 
# or provide a path to a suitable browserconfig file … otherwise leave it commented out.
BrowserConfig = "/assets/favicons/browserconfig.xml"

# Path to the 512x512 pixel image.
# This goes in the browserconfig.xml file but you can store the value here for reference 
x150 = "https://moondeer.blog/uploads/2021/c405a6363e.png"

# The tile color behind the 150x150 pixel image.
TileColor = "#DA532C"
```

You can head back to [https://realfavicongenerator.net](https://realfavicongenerator.net) and check your site. When everything is properly in place, you get mostly green results like those below.

![Checker 1](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/checker_1.png)

![Checker 2](https://raw.githubusercontent.com/moonbuck/plugin-favicons/main/images/checker_2.png)

I went one step further for sh$ts and giggles, converted the `favicon.ico` file that was in the favicon package I downloaded into a base64 image using [this converter](https://base64.guru/converter/encode/image/ico "Image Converter"). If you feel like doing that, select "plain text (just the image)" and paste the output as a string value for `Favicon.icoBase64`. You’ll end up with something kinda like:

```TOML
# Parameters derived from the use of https://realfavicongenerator.net
# to generate favicon assets. 

# General theme color for the site (used by Mobile Safari in iOS 15 to paint the background of the status bar).
ThemeColor = "#516189"

[Apple]

# Path to the 180x180 pixel image.
TouchIcon = "https://moondeer.blog/uploads/2021/0ac094eaf0.png"

# Path to the Safari pinned tab SVG file.
# Manually copy/paste your SVG content into static/assets/favicons/safari-pinned-tab.svg 
# or provide a path to a suitable SVG file … otherwise leave it commented out.
SafariPinnedTab = "/assets/favicons/safari-pinned-tab.svg"

[Favicon]

# Path to the 32x32 pixel image.
x32 = "https://moondeer.blog/uploads/2021/6e917ca7cb.png"

# Path to the 16x16 pixel image.
x16 = "https://moondeer.blog/uploads/2021/cde89be874.png"

# Path to regular image. If you upload an ico image it gets converted to jpg
#ico = "https://moondeer.blog/uploads/2021/d7e47b4fbb.jpg"

# Base64 ico image.
# You can convert .ico to base64 here: https://base64.guru/converter/encode/image/ico
icoBase64 = "AAABAAMAMDAAAAEAGACoHAAANgAAACAgAAABABgAqAwAAN4cAAAQEAAAAQAIAGgFAACGKQAAKAAAADAAAABgAAAAAQAYAAAAAAAAGwAAAAAAAAAAAAAAAAAAAAAAAIBZSoJcTIJcTIFcTIJcTYRdTYJcTH9aS35ZS39aS3tWR3tWSH9ZSoBaS4BaS3xXSX1XSYFbTINdTYljUoxmVYliUo1pW41rYoVgUoVgU4RfUY9uZJFwZn5aTXxYS4BdUYFeUoNfUYZiU4dhU4dhU4diVYpnW4RfUINdT4VfT4NeT4ZhU4VgUoFcTYBbTIJdToFbS4NdTYReTYJcTIBbS4JcTYNdTYBbS39aTH5ZSn9ZSnxXSH5YSXxXSX1YSX1XSXxXSYBaS4ReTohiUotlVIdhUYplWJp7do1oWoljVIdhUZV0a41sZH9aS35ZSoBaTIRfT4VfUIhiUYhhUYdgUIZhUYpnW4JdToVgUYVgUIRfT4ZhUoNeT4FcTYBbTYNdT4NdTYJcTINdTYRdTYFbS4FcTIFbS39ZSn5YSn1YSXxXSXpVR3tWR31YSX5YSoBaS31XSYBaS4VgT4hiUIljUodhUIllWKWOkIxpXIhkVotmV5l4b4hmXINhVoBcT4NeTodhUIVfT4pjUohiUYRfT4VgUoZiVYJcTYReT4VfUIJdToReT4NdTYFbTH5ZS4FbTYVeTYJcTIFbS4JbTIJcTIJcTIBaS35YSXtWSHlURn5YSn5YSn1XSXtWSH1XSX1XSYBbTIVfToVfToVeT4NdTYVfTolkVKiRkY9tYoVjV5FybZV4dYpoXYVhU4ZgUIhiUYljUYhiUoliUopkVIdiVIVhVYVgU4RfUYNdToReUIFbTYFbTIReToFbTH1YSYBaTIRdTX9aSn5YSX1XSYBaS35ZSoFbTH1YSXlURntWR39ZSn1YSX1YSX1XSXhTRHtVR4VgUZBsXJZ0Zp9/cZp7b5x9cpx9cqaOjZRza4dmXJuBg5h5dIdkV4ZgT4ZgT4dgUIdhUYhjVYljVIhiUodhUoVgUIZhUYVhU4VgUYReT4NdTodhUoRgUH9aS39bTIBbTYVfToNdTX5YSnxXSH1YSX1XSX9ZSn5ZSnxXSXtWSH1YSX1YSYFbTIxnWJFwZqKJhrGYlLekpr2ws8CxsL2tr7yvsrWlqqqTkYxpXYppYZZ6eIdhU4VfT4NeTYVfT4ZfUIhiU4hkWYplV4VfT4VfT4dhUItlVIhkVohjVIVfT4NeToZgUINeT4BbTYFcTYNdToVgTodhUIReTYBaS3xXSH1XSIJcS4FbS35YSn1XSX1XSI5rYKeQi7usr7auvb64wsC0tsW5u8CurrWkpbGfoKycoLioramTkoplV41sYopoXYVfT4hiUodhUIZgT4VfT4hjVIdjV4llV4dhUoVgUoZhU4dhU4ZhU4hiVIVfT4JcTYRfT4ZhUoFcTYVfUIdhT4VfTYdhT4VfToJcTH1YSXxXSH5ZSoBaSn9ZSoVfUZ6AeK+doqudorqqrbefmsewqL6xs72ppr+ppbGipq2ipLevsryvsqWJg5JwZo9uZohmW4RfUYdhU4dhUoZgUIVgUYZgUoRfUIZgUIVgUoRfUYdjWIZiVohnX4hlWIVfUINeT4RfUIJeT4BbTYRfUIhhUYhiUIZhT4VfToVfToBaSn5YSYJbTINdTZR0arajpcK3u6yamLGfnsCopL+loMSxs8Gwsa6am6yZm6eWl6KMiK6em8u8uKuMgph6d4hoYIdkWYNeUIZgUYZgUoReUIVgU4VfUoVfUIReT4RfUIVfUYZiVodkWYhpY4hlWYReToReT4JdT39aTH5ZS4JcTohhUoReTYJdTYFcTIFbS4BaSoBaSodhUaaIf7qmpsKurMewqbWclqyQiaaIgqqNiqeKha+QiLeYkcOpo7+no6iRkrCXkcOtqauRjoxrYoZkWoVgU4NeT4ReT4ReUIVhU4VgU4RfUYVfUYdiVIVgUoZiVodkWodkWI5zc4lnXoZgUYReT4ReT4FbTIBbS4NdToVeT4JcTYJdTX1YSXxXSYBaS4NdTqqNhL6mo7GTi6mJgaOCeKSCd5x6cZRzaJt4a6B+dKGAdquLg7ecma+UkK+Yl6+TjbCWkaCEgIlmW4RhVYNeUIJdT4VgUYVgUoZhVIVgUoVgVIViVohlWohjV4dlW4dlWohnXpB1dZBtYoljU4RfT4ZfUIZgUIJdToFcTYReT4JcTIBbS35ZSn9ZSoFbTKeIfriem6yMg519dJV2bqSDeaaFfJR0bZJza5h3bqB/d6WGf6SFfqiLhaSGfsCjm66Ph5+Cf45vZ4ViVoVhVYRfUYJeUIRfUoNeUYRgU4diU4ZiVoViV4hlWohmXIZkWYpqY4lqZIxuaZFxaodhU4ZgUYVgUYJdToJdToFcTYReT4NdTYNcTX5YSX9ZSpl3bLyjobOWkKSDeo5uZZZ2b6eGfZt7c5BxaZNxZ5h4cKSDeqSFgJ5+damJgK+Vkrqgn6mOi4loX4NfU4RhVYRfU4VgU4RgU4ZjWIVgU4ViVollWYllV4ZjWIhmW4hnXYhlWYttZ4ZkWo1uZ5JxaIdiVYVfUIZgUYVfUIFcTYNdT4NdToZgT4VfToJbTIhkVbebmKqNiJ19dZFwZpFwZ5l4b519dZd1bJd3b5R1bqGCequMhKOEfqKDfKWHgKyPiracl5N0aYRfUIViVoNiWIRhVodjVoZiVoNgVIRhVoRhVodkWYhkVodlW4hnXYhoYItqYIxrYYZlW5FybJRyZ4ZhU4VfUIhiUopjU4ZgUYNeT4VfT4ReToRdTYVfTqSFe7KYlp5+do9vZ5+Be6KDfZNza5V2cJp7dKeLiKqNh6eGfaSCeJx8dKGCe6GEf6SMjZl6coRiV4RhVodnYYVjWoZjV4dlWoNgVINeUIVjWYRhVoViV4ZkWYdmXIhnXohoYI5vZ45uZ5F1cpR3c49rX4NdToZgUIVfT4dhUYReTn9aS4ReToBaS4FbTI5pW7SZlaeLh5t6cZh5caiLhqeKhpl8dpt8dquSkLqin7qhnqWHgKCDfqSHhaeLiK2RjJJ0a4JgVYNhV4ZnYoZlW4RiV4ZmXYNhVoBdUYRgU4dmXIRiVoVmX4VjWIZkW4dmXohmXJR4dY5waZJ2cpFxaYhjVYRdToNdToVfToljUYVfUH1YSX9ZS4dhUIVgT6ODeKyPiZ1/eqKDfLmZj7qioKOJh5p9d6aKhLCUjLeclK6YmJ5/dqiOi66SjaGAdoppXYhoYodlXYZjVoViV4VhVIRjWYdoYoNiV4FfVYNhWIZmXodnX4doYohoYYloX4pqYY9waJh8eZBwZ5N0bZBvZoRfUYRfUIVgUIpkU4pkU4RfT39ZS4NdT4VfToljU6yPibOVjKKDfKOGgcGknbmhnqySjqaLh62RjKyQiqaNhquSjqaMh7GYl5l5cYtqXY5zcY1vbIhlWodjVYdlW4ZjVodnXopvbYlsZoNhVoNjW4dpZIttaIpsZotsZYpqYYtpXZd4cJx/epR2b49sXauQiI5qXIljUohiUoxlVo9pWIhiU4dhUohhUoNdTZRyZa6Sj7GRiKKEfpx/eaKCeaKFf6aIgK6QiayNhrSWj66Siredl7acmJh5cYprY41wa4psZ4lnXYppYIloX4ttZ4prYotvaYxwbIppX4ptaIlrZYxva4tsZY91c450cohmW4xqXZl8dZZ5dJR0a6iQjrywt559cYZfT4ZgUIpkVI9qWotmV45oWItlVYZgT6WHf62RjKWHgKCDfaKGgp+AeZ+CfaqMhamLhKaIgridmcWro6KEeY9wZolsZ4doYoViV4ZkWYhmXItqYYtrY41wa490c4xwboxuaIpsZottZ4xuaY9wa45wapJ6eotqYI5pWpd7d6KGgp9/dquTkKulubKptLCRhYZgUYZgUYhjU4lkVYhjVIpkVY5oWYtlVK6TjrOama6OhK2RjKiNiaKEfa2QiqqMhaaKhqqPjLCVkbSYkaWKh5d5copqYoZlWodlWodkV4hnXopoXopqYI1uZ4xubIhoYYtsaItuaolpX4hmWo1tZZF1cJ1/eK+UjLOUirGRiLOYkbWiorezvp+Qka+mqbmel4hhUoRfT4diU4dhUYZgUYVgUYhjVJBqWreenbedmr6lnrSeoKWFe517cKOFfpx9d6OFfqiNiredm8KtqcCmpKeLh5t8dI1qXIhkVohlWohlWYpoXYxtZZBybohmXoZgUoZiVotsZJN2cZJ1b5V3cZl6crqko8K0use3uMKyssCztcnM0sLBwqaXlKykqbiioIxlVYZhUIdhUYhiUohiUoZgUYhiU5ZxYsKsq7WYjrOemrWfm6SCdp2Aep+Cfa2RjKSFfaeKhLOXk76jnsSqpp9+cqqOipFuYIlnW4lpYotqYopoXY1tZYxsZYlkV4ZgUYZjWItrYph4b6WIgKKHhZFzbJ2BfLapr7assbGfmbahmriysrq1tqiam62jqLysrI9qW4dgUYpjVIpkVIVgUYdiU4tlVph1ZsOzsa+Wkbqoo7ugl55+dpx/e6CCfayPiaKDe62QiquOiLSamKyOhJl5b5d3bYlmWYloX4pqYYxsZI9wZ5d6dZF0cJBvZotqYY1uZ45tZZ6Bfa+Xk5yBfpV3cZJzaZ1+c7GdmLCblqCCc5+LhbWsraqZmqeTjrmkn5RvYYljU4lkVIpkVYZhUodiUohiU5h1ZcC0tLegnLqmorCTjKGCfJZ1a5t8daKFgJx8dKSGgKqOi76lpKiKgJ2Ae4hlV4lmWYppX5Bxa5uAfZuAfJR1bY9waZV3b5d7eY1uZ4xqXZFwZp1/ea2UkLWdmbedlK+RhqaJfLGakLOjnqeXlbuysLqvsqyVjbmioJVxYopkVI5nWI1oWIplVY9pV4tkVJVyYritr8azsca0qq6Ri6yMg55/d56Ae6KFgZ5+d59/d6WIhLaZkpt7b6CEgJt+e5R2cY1sYZFxaZd6dZx/ep6BfKSJiKeNi5yCg41uaIlkVqODebWgo7iqrMO2uLqyu8GxtLqxta2jpbKkormvrcLBxsO8wL+vq7qlppJsXYpjVI5oV4tlVYljUo5nVo9pWJBtXa2al7qop8i7urSdnKiIgJ1/eaKFf6iKhZ+BeqSGf62Nhb6hl6+SiKCBeq6TkZl8d5JxZJJxZJNwY518cqaJgqyOh6eMiaiPkY1tZoVfUYpmWJBuYpR0apx9caSJg7SenLatuLmws6+fnLanpcG7wLmrqrekoLKbmo1mVopjU41nVopkUodhUIljU4tlU4pkU7Kbla6bnaeUk7egnrCRiJ+Fgpd8eaOFgZ5+dp6AerCWkLuckbmbk7GYlq+Vj6+VjbilpLScmbCZlruprb+sr6aIgZ2BfbCVlJh1aoFbTIZiVJFwY5l3app3aJFtX4djVJd3bK+YlLajmLOmpLOrtamVlrScm6iKgohhUIhhUYpkU4ljUYJcTYNdTYZgT4tkU62Ujrqqq6aMhrqlnrqfl6eKha2RjKaIgaWEe51/ep6IiKySjq6Xl62XmKOFfqGEfLqjnse1sLqpp8m7vq2Ym45qXKSKhbGeoZh4cIFdT6+alK6gnqSSkrWnpr+0s56Ceo1nV4pkVY5rW5Z3Z6CGeauSjrqiop59dIZgT4ZgT4ljUodhUYJcTYBbTIFcTYhiUZ19c7musbafmK+XjryglreXjKqNhqWKiKmMhaGJi6yTkrOUjbWcm66UkaWJhZx9daKCd6mMg7CbmrWlqI5tYo1sYbWema+dn4dkWKCGfKyhpJuLjqOXmqykqLu6vr23vJJwYpVyZaGEfpRzaY5qXIxmV5NxZYxpXYdhU4ZfT4RdToFcTYBbTH9aTIFbTY1nV5JtXrmvsrWkorKYjsCtqridl6uPiq+XmK6Wk6KLiq+YlsKlnbylpK2Vla2VlbKWkLOYkqqNh7WdmpZ0Z5R0aK6VkaqPh7mjoZJuX76vr7ixsKyjoKigpJuOlaWZn7u5xJh8dKqRkr20v8i6ubmmp6eSj5Z0Z4xoWoZiVYVgU4JdToJdToJdToNdT4VfUYxmV4xnV62alrmxt7KVisGqorWkpLWgn7Scm6eMiZ6EgK2Ukq2QiqSEe5x6cK6Tj8OooLeXi62UkaiPi5NwYradmK+TjpBxZ62Vkpt4a7+xsb23tK+np5uOkpyEg6WPjcC9w6OIgqSOj7qzurSrrqqYmrGgpaOFfZBwZ4tnWodiU4VhU4ZiVYFcTn9aTIJfU49pWo5qWph3a8K6vaybmZt/eKiXm6KSlqWTlbShoayWk6WJg5d5dZp5cpp6c5+BfLCTj7CUjrqinpZ1aaWHgLKVj7GSjIxqX56Dfpp4bbiio66ZmqCJh5B1c6yQjbGXlLGhorSdlqCGf768xK2jpqKTmKePj4xoWodkV4xuaYViVoNdT4FdUIRiWYFfVYBdUodhUodiUopjU7WemLe1waiWmK6WlKCHg7anpbyxrrWjnaB/cpR2cZV4dZx+epV3cZ1+ea6TjrCUjJJxZJx+eZh6dbGUjpFvZIhoYJFxabCZmq6Zm6OLipqBgK2RkL6jn62Tjb6ztKyTirWssbSrsKmdpaGDfYFbTIVgUYxpXYppYIJdToFeVYVkXINgVJJyZ4pkVIhhUIpkVZx5a8C4v66kqrCVkaOKh7Win6+gn7WmqLGRhZR2cJFzbIxuaZB1dZ+BfKeGeqWEeZZ0aKKEfpV3cKGCepNxZ4ZiVZR1bZRyZ5+Bd6aNhqKKh6iOiq+VkqyRja+fo7KkpsKwrsS5v7mjo4pmWIBaTIZgUoZgUYVhU4llWYViVollWI1rYIpnW45oWItlVY9pWpFsXauQhsG+xKaVk6OHgLmloayen62iqq6WlZ+AeJN0bp6Cfpt+epp8dKqJgLKRh6CAdaiJgZRxZJh4cIxqXqWDd8GpqKyQi5JwZIhkVodjV5V0aZ+BeaCDfKWFeqiMg8SrpsOuspl3bIBZSn9ZS4dhUYZhUoRfUY1sY4poXYxoWZJvY5FxaItmV5JtXpVyY49rXZBsXbagmb65uquVkaSMiLmsrrSgnKSKhqOIgpx+ebKTjKCBe5V4dZ5/d7GRiLiclLCSi5h4b56CfZFuY7OUjbScnL+qqrGYkZR2cJJxZ5t8crmko72prbGbm6+WkrefoKGAd4NdTYJcS4JcTYdhUYdiU4ZgUY1sZJR3c4hjVIllVo9rXY1pWo5pWpNvYZVyY41pWo5qW7ulnsG4uquQh7WblbGcm6eTk6aOja+XlreblLGTiLSYj7ablL+loMKmnrCRiZt7c5+AeZp4bKOHhK2QibOWkLWalKyPia+UkKiMiKOGe7CUjbWgoaiXnKiMhYVfUH9ZSoFbS4JdTIVgUIVgUYReT4lkV6SKhY1pW4JdToJdTohjVI9rXI9rW4tmV4tlVoljVJFtXsCspsCzq6+dmqOUm6iZnb+lnbeakbqgl7usrLirrL+tqrabkqqMhaaIgaiKgqOEe5x7cLKZl66RiqOFfaCCe7KUjcOnn8aurcaxsregoLyqq62TjotkVIReTYBaS4FbTIJcTYRfT4ZgUIReT4hiUq6SjJh4b4BaS4JdTolkVI9qWpFsXI5oWIxnWI9qXIplVo1pWKyQg72wr6mfp6OVm7SlpL+vq8CxsL21t7i1vriutLWinLWgm7Cfoq6fpLmioamLgbWbl6qNh6ODfJ5+damMiMGlncOsqbmko72pqqeLg4pkU4ReTYVfTX5ZSoBbTIVfT4VfUIdhUoZgUYZgUJ+BeaOJhoVgUIdiU45pWY5oWY9qW5JuX5BsXZFuYJBsXY5pWYtmWJp4a7ainLKfm8OwqMrCvby2uLqzs6aXlbKorLurqbeop7KjqLimpbagobqhnMOpobegnK+Xk6eKg66Tj7+qqLSipKuTkJZ0aIReTYNeTYNdTYFcS35ZSn9aS4NeTohhUoljU4dhUYljVI9rXaSFe4tmWIZhU5RwYJJsXZJuYI1pW5JvYZFuX4llVY1oWZBrXY9rXJFtXp5/c7OakbupqLSmqbyys7muqbmztLKmqaOOj6WJhbCUkbealLWYj8espMSurryqrLmkpLqoqrSfoZ19dYxmVoReTYNdTYFaS4NdTINdTIFaS4JbTYNdTopkVIljVIhjU4xnWZFtXoxnWIZgUYZgUpVxYpp2Z5VyY45qXJJuYI5pWolkVZBqW45qW45qW4xnWYdiU4hjVJ18cbCYkreop8C2tcC5ubWtsqycoa2SjamMhK6SjbGUj8GqqMCqq7efnqeHfpl1aIlkVYJcS4ReToJcTYFbTH9ZS4NdTYNdTINdTIVeT4VfUIplVYhjU4pkU5BqW5NvYYtnWIljVItlV41oWZRvYIxnWY1oWo1oWo5oWI9pWpBrXJBsXYljVIZhU4hjVIljVYlkVoxnV5JuXpp4aqGDd6qOhKuTkKySjayQhqaHfKKBdZl5bpRxZI5oWIVeT4FaS4JcTINdTYFbTIRdToFbTH9aS4BaS4BbS4JcTYdhUIVfUIVfUYVfUIhhUYxmVoxoWYhjVIZhUoZhU4dhUodiU4ljVI1oWYpkVYhiUo1nWI1nVohjU4hiUoReUIhjVIdiU4pkVYVfUIdhUYReToNcTYVeTYhhUYpkVY5oWIVfT4BbTH9ZSoBaS4ZgUIJcTIJcTIFcTIBaS4FbS4NdTIZfT4JcTYNdTINdTYJcTYVgT4VfT4ZfUIVfT4dhUotmV4hiU4RfUINeT4BbTIZhUoZgUYpkVI1oWY1oWY9qW45oWYhiUoljU4tmVolkVY5pW4xnV4dhUoZgUYdiUoVfUIZgUoVfUIdhUIZgUYhiUoReT4JcTX5ZSn9aS39ZSn9ZSoVeToReTYBaSoBaSoJcTINdTYRdToNdTIJcTINdTIReTYVfT4ljU4xmVoljU4xmVotlVoZgUopkVo1oWYpmV41oWZFtXpRxYph2Z5RwYpBrXItlVYxnV5NvYJVwYo9rXI9qWohiU4hjVItmV4diU4hiU4VeT4ZgUYVfUYReT4NdToRdToBaS31XSXpVR31YSYJcTIJcTIFbS39aSoBaS4FbTIReTYNcTIRdTYFbS4JcTYVfUIliU41nV4pjUotlVYtmVolkVYljVYZhUopmV4plVo5qXJNvYJVyZJBsXYhiUoxmV5JtXpdyY5NvYZNuXo5nV5BqW45pWotmV4ZgUYhjU4ZfUIZgUIdhUoRfUINeT4JcTIJbTIFbTIBaS3xXSXxWSH9ZSoBaS39ZSn9aS4JcTIJcTIJbTINdTYNdTINcTYVfUIhiU4tlVIZhUoxmVpNtXoxnWIZhU4FcTQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACgAAAAgAAAAQAAAAAEAGAAAAAAAAAwAAAAAAAAAAAAAAAAAAAAAAACBW0uDXU2BXEyCXU2CXEx/WUt+WUp8V0h+WUp+WUp8V0mAWkuFX0+LZVSJY1OPbWOIY1WGYVKRcGeDX1N+WUyCXVCFYFGHYlKHYVKJZViEX1GFX1CFX1CFYFGBXE2CXE6DXEyDXU2CXEyBXEyBW0t+WUp9WEp8Vkh8V0h9WEl+WUp/WkuFX0+IYlKHYVGZe3aNal6LZleTcmmEYVWBXE6FX0+HYFCIYlKFYFCGYlWDXk+FX1CDXk+EXk+AW0yAW02DXUyAW0uBW0uBW0yAWkt7Vkd7Vkd+WEp8V0h7VUd8VkiEXk6JZFSKZVWKZVaegnyPbmSPcGuTdG6GYlSGYFCIYlGIY1OJY1OGYVOFYFKEX1GEXk+CXE6DXk5/Wkt/WUuEXk1/WUp9V0l+WEl/WUp8V0h8V0h9WEmEX1CLal+ZfneqkYyzn5y0oJ2xnp6pkpGObGKTdXGLaFuFX06FX0+HYVKJZFeHYVKGYFCIYlKIY1SFX1CEX0+EX1CAW02CXE6GYE6FX05/WUp8V0iAW0t+WEl/WkuTdGysmJi3qa6/s7bCtLW+rK2yo6a0p6usl5aOa2CLaV+FYFGHYVGGX0+GYVKHY1WHYVKGYFKHYlSHY1eGYFGDXU6EX1CCXU6GYFCHYU+FX06CXEx+WEmAWkqNal6tlpOyoqOzoKC9pKDDrq23o6Oxnp6olpa4qqmzmpSScmyIZVuEX1GGYFKFX1GFYFKEX0+EX1CFYFOHZFmIaGCFYFGEXk+BXE2AW0yGYFGDXk2BW0yAWkuAWkqZeGy1nJi7pJ+yl5CliIClhoCoiYOxk4y9op6tlpW0nJevlZGLamCEYFSDXk+EX1CFYVOEYFKGYVSGYlSGY1iHZluNcGyIY1SEXk+DXU6BXE2EXk6CXEx+WUp+WEmVc2e0mZOnh4CefXWkgnmVdWyWdWuhgHelhn6qjYezmJKtj4mXeXOGY1eEX1KDXlGEX1KFYVOGYlWGY1iIZVuHZlyJamKPcGuLZlmFYFCEXk+BXE6DXU6EXk2AWkqKZlixlZCpi4STcmmbe3KdfXSScmqZeHClhn6hgnupioO2nJicfnaEYVaDYFWFYVSFYVWFYVWGYlaIZViHZFmIZl2KaV+IaF+Pb2eLZ1uFX1CGYFGDXk+DXk+EXk2FX06khX2nioSVdW2cfHWWdm+ZenOjhoGmhn6kg3uhgXukiIShhYCHZVqFZFqFY1mGZFiEYFSEYVaFYleGY1iHZlyIaGCNbmaOb2mTc2yIY1WFX1CHYVGDXk+CXU2CXEyQbF6skIufgHimiICojIiafHapjYi5oJ2pjIekh4KnioWcfnaHZl2EYliFZFuFZFqEYlmCXlOGZFmGZVyGZVyHZl2KamGSdW+Rc26ObGOEXk+EX0+IYlGCXU5/WUuFX06ff3SsjoajhH68n5mulJGmioWtkYqsko2ojompjYmVdWuLbmiJaF+HY1eGY1iHaGGIamSDYliGZ2CJa2aKa2WKaV+ScmmYenWTc2ibe3GHYlKIYlKNZ1aGYVKGYFGKZVaoi4Sqi4Sfgnyig3ymiYKrjYaylY61mpKqjoiSdG2JamOJaWGKaF+KamOMbmiMb2uLbGSKbGeMbmmOcWyNcW6KaFyXeXOZenOlj5GzoKKOaFmHYVGMZ1eLZleNZleWcmaxl5SrjoamioajhX+pi4WnioWwlZKzmJGYenOJaWKGZFmHZFqJaF6LbGSNb2yKa2aLbWiKamKMbWWUeHKihX6pioCtkoyyn5+qoKezpKaWcmSFX1CHYlOHYVKJY1ShgHa5oJy4oJ2pjIWff3ejhX+lh4Gyl5TBqaWuk46aenGKZlmIZlyKaF2NbmeKaGCFYFOKaWGUdm+Ye3WegXq7q6+/sbK9rqzEwsaupKOypqmbem6GYE+IYlKHYVKIYlOpjIK4op24opyoioGcfnmoioWmh4CtkYy3nJijg3qWdmuKZ1yLa2OObmWRc22ObGOKaF6MbGSfgnujiIWWd3Ghhn+wnpqpkYasnpuuoaGwn52ggHaIYlKKZFWGYVKJY1OmjIO+ray5opuniIGaenKgg32fgHmoi4exlpCegHmObWKLamCTdW+bf3uXeXOZfHeVeHWMal+dfnavmZe3o6K0nJevmZKwn5uypqW9s7W1oZ2hgHaLZFSNZ1eLZlWNZ1adgXa7qqm/rqurjYaegHqjhoGggXqpi4S1l42khn+kiIWUdWyTcmibfHSni4WojYudgoGIZFeVc2ifhH6okIuynp+2rLKzpqS7sbK9tLa6p6aaeG2KZFOLZVSIYlGMZlSYd2mzn5+umZe0mZKhhYGjhYCfgHinjIe2mpKxl5WqjoiznJi3oJ26p6ixmZifgnyqj4yHY1WUdWmcgHaihnuVdWmZeW+nj4Ssm5eunJ2ymZeQa12IYVGJY1KDXU2EXk6QbF2zoaCxmpS5oJexkoqojIemiYSljIyyl5Kvl5eihYCnioGzm5W4pqeVd26lioSljoyTdWqqnJyjlpizrK+1q6ySb2KYd22WdWqXdmqce3KJZFaFX0+EXk6AW02AW02MZlaqlpK1op+8pp62nZmvlpWnjoyqkZC6npirkY6wlpO0l4+tk46fgHejhn2njYeojIWvmpS3sK+mnZ+ejpK0rLKgh4K2qK+7rK2rlpWWdmuJZViFYFKDXlCCXE6DXlCNZ1iafHG4rK2qkYuqmZuqmJmtmJanjIidf3mbenKpjIa1mJGtk42dfXSzl5KZenKbfXavl5Swn6CZg4KojYq0o6OqkYuzqa2soaWnkpSScGSLamGGYlWDXlKCX1SBXlOIYlKLZVWxnpyvo6moj4yum5m4q6mqjYSUdnCVd3KWeHSpjIakhHqZenOgg32benGJaF+ghH+qkpCgh4axlpSzm5e1paS1pqa3rbOfhYGDXU6JZViHZFiCX1OGY1qLaV6MZlWMZlacfG+5rrCoko+vmZWvoaSvmJeZenOWeXSYfHikhHuoh3yignqYd22UcmiignipjYeVdGmRcGWdf3eliYOojom9pqO7pKSLZ1mBW02GYFGHY1aKaF2NaVyPbmOOaFmTb2CQbF2jh3y5rKurlZC2o6Gqk5ChhoOtj4qjhH2khn+4m5OylI2cfXWYeG6tkIu5oZ+qj4mZenKihX21npyvmZiwmJWUcWWBW0qDXk6HYVKHYlSUdm+KZliKZVeMZ1iQa1yOaVqKZVWni4K8q6SvnJuol5q1nJe4n5i5pqO8qKW4nZasjoekhn+ff3WskYypi4SpjIW5nZa9pKK3n5y1oaGZ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"

[AndroidChrome]

# Manually copy/paste your site.webmanifest content into static/assets/favicons/site.webmanifest.txt 
# or provide a path to a suitable mobile app manifest file … otherwise leave it commented out.
Webmanifest = "/assets/favicons/site.webmanifest.txt"

# Path to the 192x192 pixel image. 
# This goes in the webmanifest but you can store the value here for reference 
x192 = "https://moondeer.blog/uploads/2021/a893f9ebc9.png"

# Path to the 512x512 pixel image.
# This goes in the webmanifest but you can store the value here for reference 
x512 = "https://moondeer.blog/uploads/2021/e4cfc0ba68.png"

# Theme color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
ThemeColor = "#516189"

# Background color to use in the webmanifest.
# This goes in the webmanifest but you can store the value here for reference 
BackgroundColor = "#516189"


[MSTile]

# Manually copy/paste your browserconfig.xml content into static/assets/favicons/browserconfig.xml 
# or provide a path to a suitable browserconfig file … otherwise leave it commented out.
BrowserConfig = "/assets/favicons/browserconfig.xml"

# Path to the 512x512 pixel image.
# This goes in the browserconfig.xml file but you can store the value here for reference 
x150 = "https://moondeer.blog/uploads/2021/c405a6363e.png"

# The tile color behind the 150x150 pixel image.
TileColor = "#DA532C"
```
