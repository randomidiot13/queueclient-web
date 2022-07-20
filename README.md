# queueclient-web
This is designed to be an easily accessible method of displaying the speedrun.com verification queue for a particular game. It can be used both by the moderators/verifiers who examine the runs, as well as the runners who submit them.

This project originated as [queueclient](https://github.com/randomidiot13/queueclient), which works similarly to this but is written in Python. I ported it to HTML/JavaScript for ease of access, though a few features, such as verifying directly through the client, had to be removed. Inspiration for both of these was drawn from luckytyphlosion's [custom-src-client](https://github.com/luckytyphlosion/custom-src-client) and Minibeast's [SRDC Verification Analyzer](http://167.71.107.25:5000/).

Currently, the only functionality the client has is queue-related, but I hope to branch out and offer more features in due time.

## speedrun.com API
All data is fetched through v1 of the [speedrun.com API](https://github.com/speedruncomorg/api), whose content is licensed under [CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/). The API will cache data, so fetching the queue twice in a short amount of time will return the same queue, even if runs were verified, rejected, or submitted in between. The time needed for the cache to clear does not appear to be consistent, but 30 minutes is usually enough time.

The API is also rate-limited, supposedly at 100 requests per minute, but this does not appear to be consistent either. The number of requests needed to fetch the queue for a given game is `ceiling((numberOfRunsInQueue + 1) / 200) + 2`.

## Sorting
The queue is sorted first by `run.date` (the date the run was done on), then by `run.submitted` (the time the run was submitted). Older runs appear first. Sorting by a different method is not supported.

## Filtering
One of the main advantages of the client is the ability to filter the queue to only display runs with a certain category and with specific subcategories. Each category/subcategory combination is named with the `full_category` function, which returns `Category[ - Subcategory1, Subcategory2, ...]` for full-game runs and `Level - Category[, Subcategory1, Subcategory2, ...]` for individual level runs. The queue can be filtered by any category/subcategory combination that appears in the queue, and this filtering defaults to `All Categories`, which is special-cased to display the entire queue.

Note: For games that have a category called `All Categories` (which at least 20 games do have) that does not have any subcategories, attempting to filter by that category will almost certainly fail. Either the entire queue will be displayed, or the client will break altogether.

## Flagging
Several games have built-in functionality to flag runs that meet certain criteria. Flagged runs will have a highlighted letter to the right of the link, and hovering over the letter will display the reason for which the run was flagged. Currently, flagging exists for the following games:
- Celeste
- Minecraft: Java Edition
- Minecraft: Java Edition Category Extensions
- Subway Surfers

Flagging for additional games may be added upon request.
