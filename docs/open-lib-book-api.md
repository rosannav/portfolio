# Book Search API
!!! info "Source material"
    [https://web.archive.org/web/20260707041106/https://openlibrary.org/dev/docs/api/search](https://web.archive.org/web/20260707041106/https://openlibrary.org/dev/docs/api/search)

**URL:** [https://openlibrary.org/search.json](https://openlibrary.org/search.json)
## Call parameters
At least one search parameter (e.g., `q`, `title`, `author`) should be provided to return meaningful results.

| Parameter  | Type                   | Description                                                                                                                                                                   | Example             |
| ---------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `q`        | string                 | General search across all fields. Supports plain text. See [Advanced search syntax](#advancted-search-syntax).                                                                | `the hobbit`        |
| `title`    | string                 | Search for works matching this title.                                                                                                                                         | `the hobbit`        |
| `author`   | string                 | Search for works by this author.                                                                                                                                              | `tolkien`           |
| `subject`  | string                 | Search by subject or topic                                                                                                                                                    | `dogs`              |
| `language` | string                 | Search by language. Excludes editions in other languages.                                                                                                                     | fre                 |
| `lang`     | string                 | Prioritise editions in a specific language. Does not exclude editions in other languages.                                                                                     | fr                  |
| `fields`   | comma-separated string | The output fields to return, comma-separated. See [`fields` options](#fields-toptions) for special values.                                                                    | `*,ia,availability` |
| `sort`     | string                 | Sort order. **Default**: relevance. See [`Sort` options](#sort-options) for required parameters that need to be requested in `fields`. Overrides `lang` (observed behaviour). | `relevance`         |
| `limit`    | integer                | Maximum number of results to return. Default: 20.                                                                                                                             | 50                  |
| `offset`   | integer                | Skips the first N results. Default: 0. Overrides `page`.                                                                                                                      | 20                  |
| `page`     | integer                | Skips to next page, with N results per page (as set with `limit`). Alternative to `offset` for pagination. Default: 1.                                                        | 3                   |
!!! info Language codes
    - `lang` uses a 2-letter ISO 639 language code.
    - `language` uses a 3-letter ISO 639 language code.
    
    See [here](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes) for a complete list of language codes.

### `Fields` options
Specify which output to return within the `fields` parameter. 

!!! note
     The Search API returns roughly 30 fields by default. For most use cases, we recommend using the `fields` parameter to request only the data you need, keeping responses small and fast.
     Will return [default output](#search-response) if left empty. If specific output parameters are requested, it will only return those parameters and **not** the default.

```bash
curl 'https://openlibrary.org/search.json?q=the+hobbit&fields=title,author_name,first_publish_year,cover_i,key,ebook_access'
```

It is also possible to requests information specific to editions, such as `editions.title`.

```bash
curl 'https://openlibrary.org/search.json?subject=dogs&fields=key,editions,title,author_name,editions.key,editions.title,editions.author_name'
```

| Parameter      | Description                                                                                                                                                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `*`            | Returns [all fields](#list-of-all-known-response-parameters).                                                                                                                                                                                      |
| `key`          | Returns works key. Necessary for requesting information on `editions`.                                                                                                                                                                             |
| `editions`     | Returns information for the first edition listed for a work based on the work `key`. First edition listed depends on sorting (see `lang`and `sort`).                                                                                               |
| `ia`           | Returns Internet Archive IDs. Necessary for requesting `availability`.                                                                                                                                                                             |
| `availability` | Returns information for the first item in the work's `ia` field. To return availability for the specific edition, include `editions` in `fields`. If `editions` has also been requested, `availability` will be shown for that particular edition. |
!!! note
    Support for returning multiple editions via `editions.row` or `editions.start` is planned but not currently available.

### `Sort` options
Options for sorting works. Some sort options require a specific parameter in `fields`.

```bash
curl 'https://openlibrary.org/search.json?subject=dogs&sort=new&fields=first_publish_year,key,title,author_name'
```


| Sort value                   | Direction  | Description                                   | Required `fields` parameter |
| ---------------------------- | ---------- | --------------------------------------------- | --------------------------- |
| ***General***                |            |                                               |                             |
| `relevance`                  | desc       | Default. By search relevance.                 | *NA*                        |
| `new`                        | desc       | Newest first by first publish year.           | `first_publish_year`        |
| `old`                        | asc        | Oldest first by first publish year.           | `first_publish_year`        |
| `editions`                   | desc       | By number of editions.                        | `edition_count`             |
| `title`                      | asc        | Alphabetically by title.                      | `title_sort`                |
| `key`                        | asc / desc | By Open Library key.                          | `key`                       |
| ***Metrics***                |            |                                               |                             |
| `rating`                     | asc / desc | By average rating.                            | `ratings_sortable`          |
| `readinglog`                 | desc       | By number of readers tracking the work.       | `readinglog_count`          |
| `want_to_read`               | desc       | By "want to read" count.                      | `want_to_read_count`        |
| `currently_reading`          | desc       | By "currently reading" count.                 | `currently_reading_count`   |
| `already_read`               | desc       | By "already read" count.                      | `already_read_count`        |
| ***Ebook access***           |            |                                               |                             |
| `ebook_access`               | asc / desc | By ebook availability.                        | `ebook_access`              |
| `scans`                      | desc       | By number of Internet Archive scanned copies. | `ia_count`                  |
| ***Library classification*** |            |                                               |                             |
| `lcc_sort`                   | asc / desc | By Library of Congress Classification.        | `lcc_sort`                  |
| `ddc_sort`                   | asc / desc | By Dewey Decimal Classification.              | `ddc_sort`                  |
| ***Random***                 |            |                                               |                             |
| `random`                     | asc / desc | Deterministic random sort (seeded).           | *NA*                        |
| `random.hourly`              | asc        | Reshuffled every hour.                        | *NA*                        |
| `random.daily`               | asc        | Reshuffled every day.                         | *NA*                        |

### Advanced search syntax
The `q` parameter accepts Solr fielded syntax for precise queries:

- `q=subject:dogs`
- `q=title:lord`
- `q=author:tolkien`

This is equivalent to using the dedicated `subject`, `title`, or `author` parameters, but supports additional boolean operators and escaping rules. See [Open Library Search How-To](https://openlibrary.org/search/howto) for complete details.
## Response parameters
!!! note
    All parameter descriptions are derived from inference by comparing the Open Library and Internet Archive webpages with JSON response. 
    
The Search API returns a single JSON object with metadata about the query and an array of works that matched the search call.

```json
{
    "numFound": 21,
    "start": 0,
    "numFoundExact": true,
    "num_found": 21,
    "documentation_url": "https://openlibrary.org/dev/docs/api/search",
    "q": "",
    "offset": null,
    "docs": [
	    {
		    "author_key": ["OL2832500A"],
		    "author_name": ["Jeff Kinney"],
	        "cover_i": 7888937,
	        "ebook_access": "borrowable",
	        ...
	    }
    ]
}
```


!!! info
    Missing fields are omitted from the response rather than returned as `null`.
    
### Metadata
Metadata about the request.

| Field                   | Type             | Description                                                                                                |
| ----------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------- |
| `numFound`; `num_found` | integer          | Total number of matching results.                                                                          |
| `start`                 | integer          | Index of the first result in the current page (0-based).                                                   |
| `numFoundExact`         | boolean          | Whether the `numFound` value is exact or estimated.                                                        |
| `documentation_url`     | string           | URL to the API documentation for this endpoint.                                                            |
| `q`                     | string           | Echo of the `q` parameter sent in the request. Empty if other paramaters were used (e.g., title, subject). |
| `offset`                | integer          | Echo of the `offset` parameter sent in the request.                                                        |
| `docs`                  | array of objects | The search results. Each object is a work containing the fields listed ...                                 |

### Search response
Default response parameters. Returned inside the `docs` array (e.g. `docs.cover_i`).

!!! note "Work vs Editions level information"
    The `docs` array contains information organised at the **work** level (i.e. book; author info, first publish year) and the **edition** level (like title, identifiers, covers, etc).
    
    If `editions` is requested separately in the `fields` parameter, the request will return an additional  `docs.editions` array with information for the specific edition that was requested.
    
    `availability` will be indexed as `docs.editions.docs.availability` if `editions` is called. If `editions` was not called, `availability` will be indexed at the works level: `docs.editions`.

| Field                  | Type             | Description                                                                 |
| ---------------------- | ---------------- | --------------------------------------------------------------------------- |
| `author_key`           | array of strings | Open Library author ID (`https://openlibrary.org/authors/{author_key}`).    |
| `author_name`          | array of strings | Author.                                                                     |
| `cover_edition_key`    | string           | Cover IDs.                                                                  |
| `cover_i`              | integer          | Cover image ID (for `https://covers.openlibrary.org/b/id/{cover_i}-M.jpg`). |
| `ebook_access`         | string           | Ebook accessibility (public, borrowable, printdisabled, no_ebook).          |
| `edition_count`        | integer          | Number of editions available.                                               |
| `first_publish_year`   | integer          | Original publication year.                                                  |
| `has_fulltext`         | boolean          | Full text availability.                                                     |
| `ia`                   | array of strings | Internet Archive ID (`https://archive.org/details/{ia}`).                   |
| `ia_collection`        | array of strings | Internet Archive collections.                                               |
| `key`                  | string           | Open Library work ID (for `https://openlibrary.org/{key}`.                  |
| `language`             | string           | Available languages (3-letter ISO 639 language code).                       |
| `lending_edition_s`    | string           | Open Library work ID of lending edition.                                    |
| `lending_identifier_s` | string           | Secondary lending edition identifier (possibly for the Internet Archive).   |
| `public_scan_b`        | boolean          | Public scan availability.                                                   |
| `series_key`           | array of strings | Series ID.                                                                  |
| `series_name`          | array of strings | Series title.                                                               |
| `series_position`      | array of strings | Position of the book in the series.                                         |
| `subtitle`             | string           | Book subtitle.                                                              |
| `title`                | string           | Book title.                                                                 |

### List of all known response parameters
Overview of all parameters that may be returned.

| Parameter                                   |                                                           |                                           |
| ------------------------------------------- | --------------------------------------------------------- | ----------------------------------------- |
| `access_score`                              | `access_score_normalized`                                 | `already_read_count`                      |
| `author_alternative_name`                   | `author_facet`                                            | `author_key`                              |
| `author_name`                               | `available_to_borrow`                                     | `available_to_browse`                     |
| `available_to_waitlist`                     | `contributor`                                             | `cover_edition_key`                       |
| `cover_height`                              | `cover_i`                                                 | `cover_width`                             |
| `currently_reading_count`                   | `ddc`                                                     | `ddc_sort`                                |
| `discovery_score`                           | `discovery_score_normalized`                              | `ebook_access`                            |
| `ebook_count_i`                             | `ebook_provider`                                          | `edition_count`                           |
| `edition_key`                               | `evaluation_score`                                        | `evaluation_score_normalized`             |
| `first_publish_year`                        | `format`                                                  | `has_fulltext`                            |
| `ia`                                        | `ia_box_id`                                               | `ia_collection`                           |
| `id_standard_ebooks` (and `id_*` variants)  | `identifier`                                              | `is_browsable`                            |
| `is_lendable`                               | `is_previewable`                                          | `is_printdisabled`                        |
| `is_readable`                               | `is_restricted`                                           | `isbn`                                    |
| `key`                                       | `language`                                                | `last_loan_date`                          |
| `last_modified`                             | `last_waitlist_date`                                      | `lcc_sort`                                |
| `lending_edition_s`                         | `lending_identifier_s`                                    | `llc`                                     |
| `llcn`                                      | `num_waitlist`                                            | `number_of_pages_median`                  |
| `numFound`                                  | `numFoundExact`                                           | `oclc`                                    |
| `openlibrary_edition`                       | `openlibrary_work`                                        | `person`                                  |
| `person_facet`                              | `person_key`                                              | `place`                                   |
| `place_facet`                               | `place_key`                                               | `printdisabled_s`                         |
| `public_scan_b`                             | `publish_date`                                            | `publish_place`                           |
| `publish_year`                              | `publisher`                                               | `publisher_facet`                         |
| `ratings_average`                           | `ratings_count` (and `ratings_count_1`–`ratings_count_5`) | `ratings_sortable`                        |
| `readinglog_count`                          | `seed`                                                    | `series_key`                              |
| `series_name`                               | `series_position`                                         | `start`                                   |
| `status`                                    | `stopped_reading_count`                                   | `subject`                                 |
| `subject_facet`                             | `subject_key`                                             | `subtitle`                                |
| `title`                                     | `title_suggest`                                           | `trending_score_daily_0` (and variations) |
| `trending_score_hourly_10` (and variations) | `trending_score_hourly_sum`                               | `trending_z_score`                        |
| `type`                                      | `usefulness_score`                                        | `usefulness_score_normalized`             |
| `want_to_read_count`                        | `work_key`                                                | `__src__`                                 |
| `_root_`                                    | `_version_`                                               |                                           |