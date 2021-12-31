## Version 0.1
The base idea is to create a service that allows podcasts creators to create picks list and even track clicks on it (track clicks on site).

The podcaster/curator can add links linking to a specific episode or upload an mp3 file for it.

## Version 0.2
Sort and index all links and let a visitor search them for:
* Podcast name
* Item Category
* Item Name

## Version 0.3
....
* 
### Possible next features
* **create analytics dashboard**
* **fuzzy search for link creation**
* **host/author profile** - this can be linked to a user or created as separated entity
* **guest edit host profile** - author of an host can send timely credentials to a host to edit its info


# Details
Base idea on how to structure the data scheme.

#### User:
* `email`
* `password`
* `role` *admin*,*curator*

#### Picks
* `name` 
* `category` *(has many)*
* `url`
* `desc`
* `podcast_id` *(has_many)*

#### Podcast
* `name`
* `desc`
* `url`
* `feed_url`
* `episodes` *(has many)*

#### Episodes
* `name`
* `desc`
* `podcast_id` *(has one)*
* `hosts` *(has many)* **Ver. 1**
* `picks`

#### Category
* `name`
* `description`
* `podcasts_id` *(has many)*
* `picks_id` *(has many)*