(DONE)first we will remove the junk footer, top bar and left contents tab and only keep the .mw-parser-output and its children
(NOTE: this 'junk' can be added later natively through flutter)

(DONE) 1we can start with a sample of 1000 and then once we confirm its all good and dandy we can apply it to all the html files

## afterwards observe the file size difference
~3.5GB compared to the now ~1.6 GB
(pretty decent change if you ask me)

# Eorzea database / xivapi part 
## https://github.com/karashiiro/xiv-resources
cross check between the different websites to make sure you have the majority of the quests

## theres 90 pages, parse the quests and sort them in folders (buckets)
## Quest
https://na.finalfantasyxiv.com/lodestone/playguide/db/quest/
## Items
https://na.finalfantasyxiv.com/lodestone/playguide/db/item/
## Duty
https://na.finalfantasyxiv.com/lodestone/playguide/db/duty/

## confirm the method of testing with quest infobox is good by seeing how many files cotain the quest infobox
## then retrieve a list of all the quests
## create custom renderers for the infoboxes

## parse the rest of the documents listing down all the various different headers they might use and and the nuances

# then retrieve a list of all the items

## and repeat the steps above