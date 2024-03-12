---
author: LKH
title: "Navigating the World of Data with ServiceNow Data Filtration"
---

In the digital age, where data zips around like caffeinated squirrels, keeping sensitive information under wraps while ensuring the cool kids (aka the right people) can still get their hands on what they need, is pretty much the IT equivalent of herding cats. Enter Data Filtration by ServiceNow – your digital shepherd, guiding you through the wilderness of data access with ease and a sprinkle of security magic.

## What’s the Big Deal with Data Filtration?

Imagine you’re throwing a VIP party (aka accessing sensitive data), and you only want certain A-listers (authorized users) on the list. Data Filtration is like the bouncer at the door, checking IDs (subject attributes) and making sure only the guests who meet your specific cool factor (criteria) get in. This isn't just another layer of security wallpaper; it's a whole new security dance floor designed to work with the existing Access Control Rules (ACLs) beats.

### The Party Features

- **Data Filters**: Think of these as the guest list criteria. If you’re not wearing the right shoes (don’t have the right data within a record), you’re not getting in.
- **Subject Attribute-Based Door Policy**: This is how you decide who’s cool based on their role, group, where they hang out (IP address), or their secret handshake (subject criteria).
- **The “No Entry” Default Dance**: Unlike most parties where the default is to sneak in unless caught, Data Filtration starts with a “you shall not pass” stance, only letting you in if you meet the strict door policy.

### Getting the Party Started

Throwing this exclusive data party requires special access, specifically, the `security_admin` VIP pass. Here’s how to roll out the red carpet:

1. **Head to the Club**: Go to `All > System Definition > Plugins` in your ServiceNow platform.
2. **Find the Secret Door**: Search for the `Data Filtration (com.glide.data_filtration)` plugin.
3. **Let the Party Begin**: Click `Install`, then hit `Activate` in the pop-up to get the beats dropping.

Once the feature is live, Data Filtration rules check the guest list after the database has spun its query tracks but before the ACLs get to decide who dances. This ensures that only the right guests boogie to the data tunes.

### Debugging the Party Fouls

Afraid of a party crasher or someone who’s not mingling right? Data Filtration’s session debugging is like having instant rehearsal, showing you exactly who will get in, who will get bounced, and why. This is a game-changer for troubleshooting and making sure your party remains exclusive.

## Setting the Scene

To curate the perfect guest list, you’ll need to set up Data Filtration records that define who gets past the velvet rope. This involves specifying the Data Filter and Subject Attribute conditions. It’s like deciding whether you’re going for a black-tie affair or a superhero costume party – it sets the tone and ensures the guests fit the vibe.

If you haven't configured Data Filtration records for a specific table, access is open to everyone, provided they meet the usual ACL validation criteria and other requirements.

## Wrapping Up

ServiceNow's Data Filtration feature is your backstage pass to a world where data access is as exclusive as a Hollywood after-party. It keeps the riff-raff out, ensures the VIPs can always get to the bar, and makes sure that your data party is the talk of the town for all the right reasons. So, roll out the red carpet and let Data Filtration keep your data safe, sound, and in the right hands.
