# Extra Instructions

Rules the LLM follows when it writes SQL for `listings`.

- `price` is the nightly price in U.S. dollars. When the user asks what something costs, use `price` and round money to whole dollars in the answer.

- When comparing or summarizing prices, report the median as well as the average, because a few very expensive listings pull the average up.

- Always say which city or cities a result covers. If the user does not name a city, group by `city` or say the answer covers all three.

- `host_is_superhost` holds the text values 't' and 'f'. Leave out rows where it is NULL when comparing superhosts to other hosts.

- `host_since` and `instant_bookable` are empty in every row. If asked about them, say that data is not available instead of running a query.