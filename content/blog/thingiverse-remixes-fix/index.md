---
title: "Thingiverse's Remix function is broken. Here's how to fix it"
date: 2021-02-01T12:13:00+01:00
draft: false
categories: ["blog"]
tags: ["3d-printing"]
---

[Thingiverse](https://www.thingiverse.com/) is becoming abandonware, which is extremely sad since it hosts countless useful 3D designs that can't be found anywhere else. Fortunately [some effort is being made](https://www.reddit.com/r/DataHoarder/search?q=thingiverse&restrict_sr=1) by the data hoarder community to backup Thingiverse in case it goes offline forever (or becomes even more broken).

If you've recently uploaded something on Thingiverse, you might have realized that one of the most important features of the site, remixes, is [badly broken](https://www.reddit.com/r/thingiverse/search/?q=remix&restrict_sr=1). Together with the [terrible integrated search engine](https://www.reddit.com/r/thingiverse/search/?q=search&restrict_sr=1), it becomes quite difficult to find stuff.

{{< figure src="box.png" caption="I'm sure I've seen boxes on Thingiverse before." >}}

If you try to upload a remixed design on thingiverse, you will find that it's not possible to link the original source thing as a remix, or at least it doesn't work for me, and many others. This is bad because your thing will not come up under the "Remixes" tab in the page of the original thing, making it less discoverable. Not to mention the missing attribution to the original work.

{{< figure src="3dbenchy.png" caption="Even 3DBenchy doesn't exist." >}}

# Workarounds
## 0. Upload to other services
Try not to keep your designs on Thingiverse alone. There are [plenty](https://www.prusaprinters.org/) of [better](https://www.myminifactory.com) [working](https://cults3d.com) [alternative](https://pinshape.com/) [platforms](https://www.cgtrader.com/) to choose from.  
I keep all my designs in a git repository on [GitHub](https://github.com/Bonnee/3d-models) and I'm trying out [PrusaPrinters](https://www.prusaprinters.org/social/56277-bonnee/prints) as a Thingiverse replacement.

### 1. Use the "Remix It" button
If you are creating a new thing just use the "Remix it" menu from the source's page. The upload page should come up with the remix sources already filled in.

{{< figure src="remix-it.png" caption="As simple as that." >}}
This works well if you are uploading a new thing, but what if you already have an uploaded design and want to add a remix source to that? or what if you want to reference more than one remix source on your design? If this is your case, keep reading.

### 2. HTML hackery
You can edit the HTML code on Thingiverse's edit page to manually include remixes. Here's how.
> Screenshots are taken from Firefox, but the procedure on Chromium-based browsers is identical.

First, open the edit page of the thing you want to add remixes to. (create a new thing using [workaround 1](#1-use-the-remix-it-button) or just edit an existing design). If the thing you're editing doesen't have a remix source, select "This is a Remix".  
Now in the edit page you should see something like this:

{{< figure src="thing.png" caption="Just right click on the name." >}}
The trick consists of manually inserting the HTML form entry for remix sources. To do that you can right-click inside the "Remix Source Files" box and select "Inspect Element" from the menu. You will now see the HTML code of the element you clicked on.  
If you don't have any remixes to click on, just right click on the placeholder text that's in its place.

{{< figure src="source.png" caption="You enetered the matrix now." >}}

Now you need to look around for a line similar to this one:
```
<input type="hidden" name="sources[xxxxxxx]" value="xxxxxxx">
```
If you can't find it don't worry. Just select any line.  
right-click on it and select "Edit As HTML". A textbox containing that line should appear.
{{< figure src="html-edit.png" caption="Double check what you're doing here to avoid confusion later." >}}
Duplicate the original line and substitute the numbers in the `name` and `value` properties with the thing id of the thing you want to remix. The thing id is the last part of the URL of the thing's page.
{{< figure src="thing-id.png" caption="" >}}

When you're done editing and everything looks good, click away from the textbox area to confirm the changes and go ahead and save your thing. Hopefully the right sources should be linked now and life will be good again.

{{< figure src="done.png" caption="Done!" >}}

You can repeat these steps indefinitely to add as many remixes as you want.