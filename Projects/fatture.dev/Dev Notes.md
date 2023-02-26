# Quick ones
- to handle the profile of the user have a read at [this article](https://dev.to/skrix/devise-user-profile-1f0h)

# Thoughts on features
#### Exchange rates
Thinking to start with [open exchange rates](https://openexchangerates.org/) since seems the most generous.

**Simple idea to save some $** - you just need to save **the rate** and store it in the database. You don't have to send request for each field.

The user will see how dated, setup a job for this each hour, the rate is and it'll be able to update it, the saved date will  be written in the global database and used as cache to save new requests.