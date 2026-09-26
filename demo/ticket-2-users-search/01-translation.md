# Translation — Users Search Broken

## Client said
> "I typed Jill Mosciski in the search box and got nothing. But she IS there — I found her by scrolling through the pages. Same thing when I paste an email… like jill_zulauf74@hotmail.com — no results."

## Client means (technical)
The search filter on the Users table is not matching rows correctly — searching by name or email returns zero results even when matching records exist, meaning the filter predicate is either comparing against the wrong field, is case-sensitive when it shouldn't be, or is never actually applied.

## In scope
- Fix the Users page search/filter so it correctly matches rows by name and/or email (case-insensitive)

## Out of scope
- Any other page or table's search functionality
- Adding new search fields (e.g. role, status)
- Backend/API changes (this appears to be a frontend-only filter)

## Assumptions
- Users data is loaded client-side (mock data or already-fetched); the filter runs in the browser
- The single "Filter users" input box should match username, full name (first + last), and email — case-insensitive substring
- No new search fields; Status/Role filters are not in scope

## Questions
_All answered. Approved Sep 26._
1. Substring match, case-insensitive. ✓
