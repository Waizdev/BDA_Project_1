# Cleaning Decisions Log

## Duplicates

- Fully identical rows: 44
- Duplicated post_ids:  60
- Decision: **PARTIAL_DUPLICATES_KEEP**
- Detail: 16 rows share post_id with DIFFERENT content — keep them; may be cross-platform re-posts.

## Case inconsistency

- **platform**: 'FACEBOOK', 'Facebook', 'INSTAGRAM', 'Instagram', 'LINKEDIN', 'LinkedIn', 'TIKTOK', 'TWITTER', 'TikTok', 'Twitter'
- **country**: 'Australia', 'Brazil', 'Canada', 'France', 'Germany', 'India', 'Japan', 'Mexico', 'South Africa', 'UAE', 'UK', 'USA', 'australia  ', 'brazil  ', 'canada  ', 'france  ', 'germany  ', 'india  ', 'japan  ', 'mexico  ', 'south africa  ', 'uae  ', 'uk  ', 'usa  '
